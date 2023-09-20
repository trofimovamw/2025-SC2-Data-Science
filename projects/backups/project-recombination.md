Freie Universität Berlin, Robert-Koch Institute

Practical course: SARS-2 Bioinformatics & Data Science

Max von Kleist, Martin Hölzer

# Project X

## Phylogeny, recombinants, and ancestral reconstruction

**Deadline**: 29.09.2023, 15:00

*The project should be worked out in groups of two or three students. Students should document their progress and present their work on Friday; 29.09.2023; 15:30am to the lecturers and students. The talk should be about 30-40 min, and allowing for 10-15min of questions.*

**a) The presentation should be uploaded via whiteboard as `ProjectX.pdf`, no later than the above stated deadline.**

An exemplary structure of the talk: 
*	Title including group composition (possibly a link to the code repository)
*	Short background (what is this good for?) & description of task
*	Methods: A step-by-step explanation of what has been done; troubleshooting
*	Results: A summary of the outcomes and analysis
*	Discussion of results; aspects that were unclear; how the workflow could be improved 
*	Outlook

**b) Codes for conducting the project tasks should be zipped into `ProjectX.zip` and uploaded either via whiteboard or made accessible via GitHub or GitLab.** In the latter case, a link to the repo should be provided on the slides and the repo must be accessible to the lecturers.

**Detailed Tasks:**

**TODO needs to be transferred into proper tasks**

Data is in the [OSF](https://osf.io/9qkz5/)

This dataset refers to the pptx slides on AY.4 and BA.1.

The international phylogeny on slide 5 can be reconstructed with the slide5-genomes* files.

The German phylogeny on slide 6 can be reconstructed with the slide6-genomes* files. Note that there is a modified alignment file which differs to the original one by simply trimming the 3' end of the alignment a bit (region with Ns).

The international phylogeny on slide 11 can be reconstructed with the slide11-genomes* files.

The international phylogeny on slide 13 can be reconstructed with the slide13-genomes* files. The spike region was manually removed from the alignment (positions 21778 to 26003 on the alignment).

RDP analysis steps: load alignment file slide11-genomes-mafft.fasta to RDP and click "Run". Then on the right hand side, up, click on the tab "Overview" and then on one of the two "events". The compared phylogenies that are shown on slide 12 will appear.
