Freie Universität Berlin, Robert-Koch Institute

Practical course: SARS-2 Bioinformatics & Data Science

Max von Kleist, Martin Hölzer

# Project 1

## SARS-CoV-2 genome assembly from Illumina reads

**Deadline**: 10.10.2025

*The project should be worked out in groups of two or three students. Students should document their progress and present their work on Friday; 10.10.2025 to the lecturers and students. The talk should be about 30-40 min, and allowing for 10-15min of questions.*

**a) The presentation should be uploaded via whiteboard as `Project1.pdf`, no later than the above stated deadline.**

An exemplary structure of the talk: 
*	Title including group composition (possibly a link to the code repository)
*	Short background (what is this good for?) & description of task
*	Methods: A step-by-step explanation of what has been done; troubleshooting
*	Results: A summary of the outcomes and analysis
*	Discussion of results; aspects that were unclear; how the workflow could be improved 
*	Outlook

**b) Codes for conducting the project tasks should be zipped into `Project1.zip` and uploaded either via whiteboard or made accessible via GitHub or GitLab.** In the latter case, a link to the repo should be provided on the slides and the repo must be accessible to the lecturers.

**Detailed Tasks:**

1) Download SARS-CoV-2 date sets from [https://osf.io/qu3bh](https://osf.io/qu3bh) 
2) For each data set reconstruct a genome sequence (for example: as taught on day 2).
  * perform quality-control of the raw reads and mapping against a reference genome (NCBI: [NC_045512.2](https://www.ncbi.nlm.nih.gov/nuccore/NC_045512.2))
3) Document statistics 
  * How many reads where aligned? What’s the read quality?
  * Read length distribution
  * Genome coverage plots
4) Generate consensus sequences
5) Analyze consensus sequences
  * How many ambiguous calls?
  * Which data set achieved the best consensus sequence in your eyes? Why? 
6) Prepare a talk to present the results & analysis
