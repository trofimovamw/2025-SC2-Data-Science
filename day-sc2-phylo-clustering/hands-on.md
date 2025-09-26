# Workshop: SARS-CoV-2 phylogeny & outbreak investigation

## Alignment and tree

In this part, we will calculate a multiple sequence alignment (MSA) and a phylogenetic tree. We will do this using the Linux system and command line interfance and online tools for tree visualization. 

The tools we will use are listed here and will be discussed in detail below:

* `MAFFT`
* `IQTree`
* `president`

__Install all tools__
```bash
# config some channels, this might be already done.
# basically helps to not explicitly type the channels
conda config --add channels bioconda
conda config --add channels conda-forge

# now we create a new environment and install all tools
mamba create -p envs/tree president mafft iqtree jalview

# activate the env
conda activate envs/tree
```

### Example data
```bash
# get example data, a collection of different SARS-CoV-2 lineages, full genomes
wget --no-check-certificate https://osf.io/wpk75/download -O sc2-genomes-diff-lineages.tar.gz
# extract the archive
tar zxvf sc2-genomes-diff-lineages.tar.gz
```

### Multiple sequence alignment

* `MAFFT`

```bash
# first we need a multiple FASTA file, which we can for example generate
# by 'cat'ing together single FASTA files, like the ones in the example-data folder
cat sc2-genomes-diff-lineages/*.fasta > all.fasta

# now we can calculate the alignment
mafft --thread 4 all.fasta > alignment.fasta
```

__Task:__ Check the `alignment.fasta` - do you see mismatches? Gaps? You can for example use `jalview` to look at the alignment! The tool is also installed in your conda env.

### Phylogenetic reconstruction

* `IQTree`

```bash
# simple usage (there are many parameters though!)
iqtree -nt 4 -s alignment.fasta --prefix phylo

# first look at the output, scroll a bit to see a tree in ASCII format
cat phylo.iqtree

# the actual tree is stored in the so-called newick format:
cat phylo.treefile
```

### Tree visualization

* `IROKI`

Go to [https://www.iroki.net/](https://www.iroki.net/) and upload a tree file in `newick` format, e.g. `phylo.treefile`. 

## Outbreak detection, clustering and infection chains

Now we want to try identify putative infection chains using SARS-CoV-2 sequences based on their mutation profile using [https://github.com/rki-mf1/breakfast](https://github.com/rki-mf1/breakfast). We will also need some additional tools to prepare the input and post-process the clustering results:

* `minimap2`
* `gofasta`
* `seqkit`
* `breakfast`
* `R with the dplyr library`

__Install all tools__
```bash
# now we create a new environment and install all tools
mamba create -p envs/chaining minimap2 gofasta seqkit breakfast r-dplyr

# activate the env
conda activate envs/chaining
```

### Example data

```bash
# get example data, a collection of different SARS-CoV-2 lineages, full genomes
wget --no-check-certificate https://osf.io/kxasc/download -O breakfast-clustering-data.tar.gz
# extract the archive
tar zxvf breakfast-clustering-data.tar.gz
# For now we only want the sequences that were in the archive, so we symlink them into our current directory
ln -s breakfast-clustering-data/input-sim-5000.fasta.xz sequences.fasta.xz
# We also need the reference genome that you downloaded in the "Genome reconstruction" hands-on
ln -s nCoV-2019.reference.fasta reference.fasta
# Lastly we need some fake metadata that we will assign to the simulated sequences we downloaded above, just for the sake of this lesson
wget https://zenodo.org/record/8355599/files/SARS-CoV-2-Sequenzdaten_Deutschland.tsv.xz?download=1 -O metadata-germany.tsv.xz
NUM_SEQS=$(xzcat sequences.fasta.xz | grep -c "^>")
xzcat metadata-germany.tsv.xz | head -n $((NUM_SEQS + 1)) > metadata.tsv
```

### Identifying mutations

Breakfast uses the set of mutations of each sequences to compute a distance measure between sequences. These need to be precomputed, and this is commonly already done because we are interested in mutations for many reasons other than outbreak detection.

For the sake of this course we will use a very fast method to identify mutations, which ignores insertions in the query (non-reference) sequence. If you have a small set of sequences or you don't mind the calculation taking longer, you could also use MAFFT (see above) to build an MSA, or even use covsonar to directly calculate the mutations given a set of seqeunces.

```bash
xzcat sequences.fasta.xz | minimap2 -a -x asm20 --sam-hit-only --secondary=no --score-N=0 -t 4 reference.fasta - | gzip > mapped-sequences.sam.gz
zcat mapped-sequences.sam.gz | gofasta sam toMultiAlign -t 4 --reference reference.fasta --trimstart 265 --trimend 29674 --trim --pad | seqkit seq --upper-case -o msa.fasta.gz 
```

Now we have mapped all sequences to the reference, and used that mapping to generate an MSA. Note that we specifically trim the beginning and end of each sequence because these regions were found to sometimes contain errors, and we pad the sequences so that the position of each mutation is correct relative to the reference when we calculate the SNPs.

```bash
zcat msa.fasta.gz | gofasta snps -r reference.fasta | gzip > snps.csv.gz
# Check the first few rows to see that the output looks reasonable
zcat snps.csv.gz | head | column -t -s','
```

### Putative infection chain identification 

Now that we have a file listing the mutations in each of our sequences, we can use `breakfast` to identify putative infection chains.

```bash
mkdir breakfast-results
breakfast --input-file snps.csv.gz --max-dist 1 --sep ',' --sep2 '|' --id-col query --clust-col SNPs --jobs 4 --outdir breakfast-results
```

### Summarize the results

The output of the breakfast is a table of input sequences IDs, and either a cluster membership or "NA" if the sequence does not cluster with any other sequence. In order to make use of this data, we need to do some downstream analysis. For this I'll use R, and the nice "dplyr" package which makes it easy to join different data sets and manipulate tabular data.

First we start R:

```bash
$ R

R version 4.5.1 (2025-06-13) -- "Great Square Root"
[...]
```

Now we can load dplyr, and our data sets, reassing the sequence IDs in our dummy metadata set, and calculate the largest chains that we found in our data set:

```R
> library(dplyr)
> chains <- read.delim("breakfast-results/clusters.tsv")
> metadata <- read.delim("metadata.tsv")
> # We don't have real metadata for these simulated sequences, so we just use
> # some existing Germany SC2 metadata and arbitrary assign it to our simulated
> # sequences.
> metadata$SEQUENCE.ID <- chains$id
> top10 <- chains %>%
    filter(!is.na(cluster_id)) %>%
    group_by(cluster_id) %>%
    summarize(chain_size=n(), ids=paste(id, collapse=",")) %>%
    arrange(desc(chain_size)) %>%
    head(n=10)
> top10
> write.table(top10, file="top10.tsv", sep="\t")
```

The next step could be to look in more detail into a single cluster. For this, we almost always need to merge in some sequence metadata. Here we will take the largest chain, and summarize the PLZ of the diagnostic labs where the samples were taken. This is the highest level of geographic detail we have for our data due to data protection restrictions:

```R
> biggest_id <- as.numeric(top10[1, "cluster_id"])
> biggest_plz_summary <- chains %>%
    filter(cluster_id == biggest_id) %>%
    left_join(metadata, by=join_by(id == SEQUENCE.ID)) %>%
    group_by(DL.POSTAL_CODE) %>%
    summarize(n=n()) %>%
    arrange(desc(n))
> biggest_plz_summary
```

Most of the sequences come from a diagnostic lab with a PLZ that is in Koeln. But is that noteworty or do they just submit a lot of sequences in general? If this was a real outbreak we would expect some association between the cluster membership and geographic location (we use DL PLZ as a proxy for this). To help answer this question we can use [Fisher's Exact Test](https://en.wikipedia.org/wiki/Fisher%27s_exact_test), to test whether the proportion of sequences belonging to the PLZ is different in our cluster than it is in the full dataset:

```R
> chain_meta <- chains %>% left_join(metadata, by=join_by(id == SEQUENCE.ID))
> fisher.test(chain_meta$cluster_id == 2, chain_meta$DL.POSTAL_CODE == 50858)

        Fisher's Exact Test for Count Data

data:  chain_meta$cluster_id == 2 and chain_meta$DL.POSTAL_CODE == 50858
p-value = 0.9284
alternative hypothesis: true odds ratio is not equal to 1
95 percent confidence interval:
 0.892013 1.132012
sample estimates:
odds ratio
  1.005186

```

We see from the results of our statistical test that the association between a sequence belonging to cluster 2 and coming from a diagnostic lab in the PLZ 50858 is not statistically significant (p-value = 0.9284), so we don't expect that this is a real outbreak. It is not surprising because we are using dummy metadata which has been arbitrarily assigned to our sequences.

You could do the same statistical test for all of the different metadata fields to see if there is any association, as well as for all of the clusters, but you will start to run into problems with multiple testing and need to consider [multiple testing correction](https://en.wikipedia.org/wiki/Multiple_comparisons_problem). That is outside of the scope of this course.
