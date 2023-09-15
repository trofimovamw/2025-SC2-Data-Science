# SARS-CoV-2 Bioinformatics & Data Science

A practical introduction to genomic surveillance using SARS-CoV-2 as an example and in the context of the FU course "SARS-CoV-2 Bioinformatics & Data Science" 2023.

## Schedule links for the workshop

* [2023-09-18 - Monday: Welcome, Linux re-cap, container, WMS](#0)  
* [2023-09-19 - Tuesday: SARS-CoV-2 sequencing & genome reconstruction](#6)  
* [2023-09-20 - Wednesday: SARS-CoV-2 evolution, mutation profiling & phenotypization](#7)  
* [2023-09-21 - Thursday: SARS-CoV-2 pathogen evolution & genome-based incidence estimation](#8)  
* [2023-09-22 - Friday: SARS-CoV-2 phylogeny & outbreak investigation](#9)  

## Instructors

* Max von Kleist (FU, RKI) and Martin Hoelzer (RKI) & many great colleagues

## Schedule

> All events are held at FU Arnimallee 6, Room 017

### <a name="0"></a> Monday, 2023-09-18
| Time        | Welcome, Linux re-cap, container & WMS |
| --          | --               |
| 10:00-10:30 | [Welcome & course intro](day-sc2-intro/README.md) |
| 10:30-11:00 | [RKI Genomic Surveillance and SARS-CoV-2](day-sc2-intro/README.md) |
| 11:00-12:00 | [Linux re-cap](day-welcome-linux-container-wms/linux.md) |
| 12:00-13:00 | Lunch break |
| 13:00-14:00 | [Container & WMS](day-welcome-linux-container-wms/container-wms.md) |
| 14:00-14:30 | Coffee break |
| 14:30-16:00 | [Hands-on & demo](day-welcome-linux-container-wms/hands-on.md) |
| 16:00-16:15 | Wrap-up & questions |

### <a name="6"></a> Tuesday, 2023-09-19
| Time        | SARS-CoV-2 sequencing & genome reconstruction |
| --          | --               |
| 10:00-10:15 | Debriefing previous day |
| 10:15-11:00 | [Sequencing (SARS-CoV-2)](day-sc2-seq-and-assembly/README.md) |
| 11:00-12:00 | [SARS-CoV-2 genome reconstruction](day-sc2-seq-and-assembly/README.md) |
| 12:00-13:00 | Lunch break |
| 13:00-14:30 | [Hands-on & demo](day-sc2-seq-and-assembly/hands-on.md) |
| 14:30-15:00 | Coffee break |
| 15:00-15:45 | Continue practical session |
| 15:45-16:00 | Wrap-up & questions |

### <a name="7"></a> Wednesday, 2023-09-20
| Time        | SARS-CoV-2 evolution, mutation profiling & phenotypization |
| --          | --               |
| 10:00-10:15 | Debriefing previous day |
| 10:15-11:00 | [SARS-CoV-2 genome organisation & evolution](day-sc2-evolution/README.md) |
| 11:00-12:00 | [SARS-CoV-2 epistasis and variants](day-sc2-evolution/README.md) |
| 12:00-13:00 | Lunch break |
| 13:00-14:00 | [SARS-CoV-2 nomenclature](day-sc2-evolution/README.md) |
| 14:00-14:30 | [SARS-CoV-2 mutation profiling](day-sc2-evolution/README.md) |
| 14:30-15:00 | Coffee break |
| 15:00-15:45 | [Hands-on & demo](day-sc2-evolution/hands-on.md) |
| 15:45-16:00 | Wrap-up & questions |

### <a name="8"></a> Thursday, 2023-09-21
| Time        | SARS-CoV-2 pathogen evolution & incidence estimation |
| --          | --               |
| 10:00-10:15 | Debriefing previous day |
| 10:15-12:00 | [SARS-CoV-2 pathogen evolution and genome-based incidence estimation](day-sc2-incidence/README.md) |
| 12:00-13:00 | Lunch break |
| 13:00-14:00 | [SARS-CoV-2 risk score via VOCAL](day-sc2-incidence/README.md) |
| 14:00-14:30 | Coffee break |
| 14:30-15:45 | [Hands-on & demo](day-sc2-incidence/hands-on.md) |
| 15:45-16:00 | Wrap-up & questions |

### <a name="9"></a> Friday, 2023-09-22
| Time        | SARS-CoV-2 phylogeny & outbreak investigation |
| --          | --               |
| 10:00-10:15 | Debriefing previous day |
| 10:15-12:00 | [Phylogeny and outbreak investigation](day-sc2-phylo-clustering/README.md) |
| 12:00-13:00 | Lunch break |
| 13:00-14:00 | [Outbreak detection & clustering via breakfast](day-sc2-phylo-clustering/README.md) |
| 14:00-14:30 | Coffee break |
| 14:30-15:45 | [Hands-on & demo](day-sc2-phylo-clustering/hands-on.md) |
| 15:45-16:00 | Wrap-up & questions |




## Acknowledgement

This course material is partly based on the following resources and on contributions from great people (no specific order):

* Martin Hoelzer, RKI MF1, content about Linux, container, Nextflow, sequencing, genomic surveillance & glueing everything together
* Sebastian "Raverjay" Krautwurst, FSU Jena, some Linux and ONT content
* Stephan Fuchs, RKI MF1, some Linux and Assembly content 
* Matt Huska, RKI MF1, automatic test script for all md code blocks using [codedown](https://github.com/earldouglas/codedown) and general help
* Workshop structure inspired by [https://github.com/cinemaparis/2023](https://github.com/cinemaparis/2023)
* Max von Kleist, RKI P5 and FU Berlin, basically most content about sequencing and SC2 data science (evolution, epistasis, incidence esitmation, ...)
* Maureen Smith, Maria Trofimova, RKI P5 and FU Berlin, content on Incidence estimation
* Hugues Richard, RKI MF1, content about SC2 risk assessment
* Matt Huska & Denis Beslic, RKI MF1, content about SC2 outbreak detection & clustering