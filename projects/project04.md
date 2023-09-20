Freie Universität Berlin, Robert-Koch Institute

Practical course: SARS-2 Bioinformatics & Data Science

Max von Kleist, Martin Hölzer

# Project 4

## Genome-based SARS-CoV-2 incidence estimation and case ascertainment

**Deadline**: 29.09.2023, 14:00

*The project should be worked out in groups of two or three students. Students should document their progress and present their work on Friday; 29.09.2023; 14:30am to the lecturers and students. The talk should be about 30-40 min, and allowing for 10-15min of questions.*

**a) The presentation should be uploaded via whiteboard as `Project4.pdf`, no later than the above stated deadline.**

An exemplary structure of the talk: 
*	Title including group composition (possibly a link to the code repository)
*	Short background (what is this good for?) & description of task
*	Methods: A step-by-step explanation of what has been done; troubleshooting
*	Results: A summary of the outcomes and analysis
*	Discussion of results; aspects that were unclear; how the workflow could be improved 
*	Outlook

**b) Codes for conducting the project tasks should be zipped into `Project4.zip` and uploaded either via whiteboard or made accessible via GitHub or GitLab.** In the latter case, a link to the repo should be provided on the slides and the repo must be accessible to the lecturers.

**Detailed Tasks:**

1) Install `GInPipe` [https://github.com/KleistLab/GInPipe](https://github.com/KleistLab/GInPipe)
2) Download data:
  * SARS-CoV-2 sequences, reported case numbers, and testing statistics for Berlin: [TODO]()
  * SARS-CoV-2 sequences, reported case numbers, and testing statistics for North Rhine-Westphalia (NRW): [TODO]()
  * Index reference sequence from Wuhan outbreak, NCBI: [NC_045512.2](https://www.ncbi.nlm.nih.gov/nuccore/NC_045512.2)
3) Explore data:
  * Create a plot showing the number of sequences per day for both federal states. The sampling dates are given in the headers within the fasta file.
  * Plot a histogram of the amount of "N"s in the sequences for both federal states.
4) Configure & run `GInPipe` with the data sets. _(Note: The run may take a while, 20-30 minutes, for the larger data sets)_
  * Visualize output (reported incidences vs. `GInPipe` incidence correlate ϕ)
5) Based on ϕ, test numbers and test positivity, compute case ascertainment rates.
