# HiC-Pro WDL
HiC-Pro in wdl format is 

### WorkFlow
![This is a alt text.](/docs/HiCPro-Diagram.png "HiC-Pro workflow diagram")

### Dependence
* [HiC-Pro](https://github.com/nservant/HiC-Pro)

### Quick Start
* Create json file - [Womtool](https://github.com/broadinstitute/cromwell/releases)
> java -jar womtool-52.jar inputs xxx.wdl > xxx.wdl.json
* Run wdl file - [Cromwell](https://github.com/broadinstitute/cromwell)
> java -jar cromwell-XY.jar run myWorkflow.wdl -i myWorkflow.json
* [WDL guideline](https://github.com/openwdl/wdl)
