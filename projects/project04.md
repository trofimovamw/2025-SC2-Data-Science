Freie Universität Berlin, Robert-Koch Institute

Practical course: SARS-2 Bioinformatics & Data Science

Max von Kleist, Martin Hölzer

# Project 4

## Genome-based SARS-CoV-2 incidence estimation and case ascertainment

**Deadline**: 04.10.2024, 14:00

*The project should be worked out in groups of two or three students. Students should document their progress and present their work on Friday; 04.10.2024; 14:30am to the lecturers and students. The talk should be about 30-40 min, and allowing for 10-15min of questions.*

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
2) Download the following data:
  * [England data in CSV format](https://box.fu-berlin.de/s/aroKcNJdfKFNbb3)
  * [Reformatted data from UK Coronavirus Infection Survey study](https://box.fu-berlin.de/s/qC8dXWQ54GPJLnD): you can find the Infection Survey methodology [here](https://www.ons.gov.uk/peoplepopulationandcommunity/healthandsocialcare/conditionsanddiseases/methodologies/covid19infectionsurveypilotmethodsandfurtherinformation) 
3) Explore and subsample data:
  * Extract entries that fall into the time period between 2020-07-01 and 2021-06-01
  * Plot a histogram of the amount of entries per day for the truncated data set
  * Subsample data to obtain uniform data distribution along the time axis (e.g. split data into bins of a some length and subsample to match the size of each bin to the smallest one)
5) Analyse both full and subsampled datasets using `GInPipe`:
  * Configure (see course notes or README in the `GInPipe` repository) and run the pipeline
6) Discuss the results and compare with UK Coronavirus Infection Survey:
  * Note that the UK Coronavirus Infection Survey provides an estimate for SC2-positive fraction of population for a time window (midpoint of each window is provided in the file in column **date**). Pick a method and obtain the (average) daily number of SC2-positives before plotting
  * Column **estimated_average_pop** is an estimate of how many people were SC2-positive in the given time window, while column **estimated_average** gives average percentage of SC2-positive population. Rescale the lower (**lower_bound_95CI**) and upper (**upper_bound_95CI**) percentage bounds of the estimate in the same way; use population size of 56.3 million
  * Make a plot with a double y-axis, with UK Coronavirus Infection Survey data (column name: **estimated_average_pop**) on one side and `GInPipe` estimates on the other
  * Compare and discuss your results
