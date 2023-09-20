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
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge

# now we create a new environment and install all tools
mamba create -n tree president mafft iqtree jalview

# activate the env
conda activate tree
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
* `breakfast`
* `R with the dplyr library`

__Install all tools__
```bash
# now we create a new environment and install all tools
mamba create -n chaining minimap2 gofasta breakfast r-dplyr

# activate the env
conda activate chaining
```

### Example data

```bash
# get example data, a collection of different SARS-CoV-2 lineages, full genomes
wget --no-check-certificate https://osf.io/kxasc/download -O breakfast-clustering-data.tar.gz
# extract the archive
tar zxvf breakfast-clustering-data.tar.gz
# For now we only want the sequences that were in the archive, so we symlink them into our current directory
ln -s breakfast-clustering-data/input-sim-5000.fasta.xz .
# We also need the reference genome that you downloaded in "Mapping and Visualization" hands-on
ln -s nCoV-2019.reference.fasta reference.fasta
```

### Identifying mutations

Breakfast uses the set of mutations of each sequences to compute a distance measure between sequences. These need to be precomputed, and this is commonly already done because we are interested in mutations for many reasons other than outbreak detection.

For the sake of this course we will use a very fast method to identify mutations, which ignores insertions in the query (non-reference) sequence. You could also use MAFFT (see above).

```bash
minimap2 -a -x asm20 --sam-hit-only --secondary=no --score-N=0 -t 4 reference.fasta sequences.fasta | gzip > mapped-sequences.sam.gz
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

```bash
$ R

R version 4.1.3 (2022-03-10) -- "One Push-Up"
Copyright (C) 2022 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

> library(dplyr)

Attaching package: ‘dplyr’

The following objects are masked from ‘package:stats’:

    filter, lag

The following objects are masked from ‘package:base’:

    intersect, setdiff, setequal, union

>
```
