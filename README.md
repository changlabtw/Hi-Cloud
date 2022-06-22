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

### Performance
<table class="MsoTable15Grid4Accent5" style="border-collapse:collapse;border:none" cellspacing="0" cellpadding="0" border="1">
 <tbody><tr>
  <td style="width:126.35pt;border:solid #5B9BD5 1.0pt;
  border-right:none;background:#5B9BD5;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif;
  color:white" lang="EN-US">Performance</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:solid #5B9BD5 1.0pt;
  border-left:none;border-bottom:solid #5B9BD5 1.0pt;border-right:none;
  background:#5B9BD5;padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif;
  color:white" lang="EN-US">Non-Parallel Azure D13_v2 x 1</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:solid #5B9BD5 1.0pt;
  border-left:none;border-bottom:solid #5B9BD5 1.0pt;border-right:none;
  background:#5B9BD5;padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal" style="text-align:center" align="center"><b><span style="font-family:&quot;Times New Roman&quot;,serif;color:white" lang="EN-US">Parallel
  Azure D13_v2 x 3</span></b></p>
  </td>
  <td style="width:70.7pt;border-top:solid #5B9BD5 1.0pt;
  border-left:none;border-bottom:solid #5B9BD5 1.0pt;border-right:none;
  background:#5B9BD5;padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal" style="text-align:center" align="center"><b><span style="font-family:&quot;Times New Roman&quot;,serif;color:white" lang="EN-US">Parallel
  Azure D13_v2 x 10</span></b></p>
  </td>
  <td style="width:70.75pt;border:solid #5B9BD5 1.0pt;
  border-left:none;background:#5B9BD5;padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal" style="text-align:center" align="center"><b><span style="font-family:&quot;Times New Roman&quot;,serif;color:white" lang="EN-US">Parallel
  Azure D13_v2 x 20</span></b></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Bowtie_Global_Mapping</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">8h:01m:51s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1h:24m:14s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">39m:52s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">37m:11s</span></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Bowtie_Local_Trimming</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:58s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:16s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:25s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:34s</span></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Bowtie_Local_Mapping</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1h:27m:51s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">6m:50s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">3m:08s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">3m:19s</span></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Mapping_Combine</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">43m:42s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">3m:35s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:55s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:41s</span></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Mapping_Stat</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">10m:24s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">2m:21s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:24s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:16s</span></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Merge_Pairs</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">33m:48s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">4m:29s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">2m:37s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:42s</span></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Mapped_Hic_Fragments</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1h:51m:29s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">12m:0s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">4m:40s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">4m:37s</span></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Merge_Valid_Interaction</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">3m:28s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">4m:11s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">3m:47s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">3m:57s</span></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Making_Plot</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:10s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:27s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:44s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:21s</span></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Build_Matrix</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:37s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">2m:0s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">2m:31s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1m:41s</span></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Ice_Normalization</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">2m:16s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">3m:22s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">2m:54s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">3m:04s</span></p>
  </td>
 </tr>
 <tr>
  <td style="width:126.35pt;border:solid #9CC2E5 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt" width="168" valign="top">
  <p class="MsoNormal"><b><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">Total</span></b></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">13h:13m:6s</span></p>
  </td>
  <td style="width:73.5pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="98" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">2h:4m:22s</span></p>
  </td>
  <td style="width:70.7pt;border-top:none;border-left:none;
  border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1h:4m:06s</span></p>
  </td>
  <td style="width:70.75pt;border-top:none;border-left:
  none;border-bottom:solid #9CC2E5 1.0pt;border-right:solid #9CC2E5 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt" width="94" valign="top">
  <p class="MsoNormal"><span style="font-family:&quot;Times New Roman&quot;,serif" lang="EN-US">1h:1m:08s</span></p>
  </td>
 </tr>
</tbody></table>

### Claimer
* You can't run this wdl file locally, it may cause error with uncorrect directory setting. Please run it though Atgenomix.
