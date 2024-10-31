# WGS-snakemake

This snakemake pipeline is used for human WGS and WES sequencing projects and supports hg19 and hg38.

## Pipeline summary

1: Quality check and trimming
  1. Raw read QC (`FastQC`)
  2. 3' adapter trimming (`Cutadapt`)
  3. Present QC for raw read (`MultiQC`)
  
2: Alignment and mark duplicates
  1. Map reads to reference (`BWA`)
  2. Mark duplicates (`Picards`)

3: BQSR
  1. Base Quality Score Recalibration (`GATK BaseRecalibrator`)
  2. Apply base quality score recalibration (`GATK ApplyBQSR`)

4: Variant calling
  1. Call germline SNPs and indels via local re-assembly of haplotypes (`GATK HaplotypeCaller`)
  2. Import single-sample GVCFs into GenomicsDB before joint genotyping (`GATk GenomicsDBImport`)
     GenomicsDBImport offers the same functionality as CombineGVCFs and comes from the Intel-Broad Center for Genomics.
  3. Perform joint genotyping on one or more samples pre-called with HaplotypeCaller (`GATK GenotypeGVCFs`)
  4. Gathers multiple VCF files from a scatter operation into a single VCF file (`Picard GatherVcfs`)

5: VQSR
  1. Build a recalibration model to score variant quality for filtering purposes (`GATK VariantRecalibrator`)
  2. Apply a score cutoff to filter variants based on a recalibration table (`GATK ApplyVQSR`)

6: Variant filtering and annotation
  1. Filter variant calls based on INFO and/or FORMAT annotations (`GATk VariantFiltration`)
  2. Variant annotation (`SnpEff and Annovar`)
7: GWAS
  1. ([`plink`](https://www.cog-genomics.org/plink/))

