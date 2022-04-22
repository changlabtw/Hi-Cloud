# HiC-Pro WDL
HiC-Pro WDL is a way to specify data processing workflows with a human-readable and writeable syntax. WDL makes it straightforward to define complex analysis tasks, chain them together in workflows, and parallelize the execution.

## Why HiC-Pro in WDL format
* Easy to understand and change specify task
* Make parallelize on cloud easier

### WorkFlow
<div align = "center"><img height = "600" width = "300" src="/docs/HiCPro-Diagram.png"/></div>

0. Fastq Partition: Split the fastq data into smaller files (Cloud Only)
1. Global Mapping: Use Bowtie2 align the reads to reference genome 
2. Trimming: Remaining unmapped reads spanning the ligation junction are trimmmed from their 3’ end.
3. Local Mapping: Align trimmed reads to reference genome again.
4. Mapping Combine: Combine global mapping and local mapping results
5. Mapping Stat: Stat the mapping quality.
6. Merge Pairs: Merge mapped R1 and R2 bam files.
7. Mapped HicFragments: Filter out invalid pairs and output valid reads
8. Merge Valid Interaction: Merge all the valid reads from different cpu outcome (sample.allValidPairs).
9. Build Matrix: Build contact matrix base on sample.allValidPairs.
10. Making Plot: Merge stat files and visualize mapping quality.
11. Ice Normaliztion: Use package iced to normalize contact matrix.

### Dependence
* [HiC-Pro](https://github.com/nservant/HiC-Pro)

### Quick Start
* Create json file - [Womtool](https://github.com/broadinstitute/cromwell/releases)
> java -jar womtool-52.jar inputs xxx.wdl > xxx.wdl.json
* Run wdl file - [Cromwell](https://github.com/broadinstitute/cromwell)
> java -jar cromwell-XY.jar run myWorkflow.wdl -i myWorkflow.json
* [WDL guideline](https://github.com/openwdl/wdl)

### Claimer
* You can't run this wdl file locally, it may cause error with uncorrect directory setting. Please run it though Atgenomix.
