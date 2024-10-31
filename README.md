# WGS-snakemake

This snakemake pipeline is used for human WGS and WES sequencing projects and supports hg19 and hg38.

## Pipeline summary

- 1. Quality check and trimming
  1. Raw read QC (`FastQC`)
  2. 3' adapter trimming (`Cutadapt`)
  3. Present QC for raw read (`MultiQC`)
  
- 2. Alignment and mark duplicates
  1. Map reads to reference (`BWA`)
  2. Mark duplicates (`Picards`)

