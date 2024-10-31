# WGS-snakemake

This snakemake pipeline is used for human WGS and WES sequencing projects and supports hg19 and hg38.

## Pipeline summary

1: Quality check and trimming
  1. Raw read QC ([`FastQC`](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/))
  2. 3' adapter trimming ([`Cutadapt`](https://cutadapt.readthedocs.io/en/latest/))
  3. Present QC for raw read ([`MultiQC`](https://seqera.io/multiqc/))
  
2: Alignment and mark duplicates
  1. Map reads to reference ([`BWA`](https://bio-bwa.sourceforge.net/))
  2. Mark duplicates ([`Picard`](https://broadinstitute.github.io/picard/))

3: BQSR
  1. Base Quality Score Recalibration ([`GATK BaseRecalibrator`](https://github.com/broadinstitute/gatk/releases))
  2. Apply base quality score recalibration ([`GATK ApplyBQSR`](https://github.com/broadinstitute/gatk/releases))

4: Variant calling
  1. Call germline SNPs and indels via local re-assembly of haplotypes ([`GATK HaplotypeCaller`](https://github.com/broadinstitute/gatk/releases))
  2. Import single-sample GVCFs into GenomicsDB before joint genotyping ([`GATk GenomicsDBImport`](https://github.com/broadinstitute/gatk/releases))
     GenomicsDBImport offers the same functionality as CombineGVCFs and comes from the Intel-Broad Center for Genomics.
  3. Perform joint genotyping on one or more samples pre-called with HaplotypeCaller ([`GATK GenotypeGVCFs`](https://github.com/broadinstitute/gatk/releases))
  4. Gathers multiple VCF files from a scatter operation into a single VCF file ([`Picard GatherVcfs`](https://broadinstitute.github.io/picard/))

5: VQSR
  1. Build a recalibration model to score variant quality for filtering purposes ([`GATK VariantRecalibrator`](https://github.com/broadinstitute/gatk/releases))
  2. Apply a score cutoff to filter variants based on a recalibration table ([`GATK ApplyVQSR`](https://github.com/broadinstitute/gatk/releases))

6: Variant filtering and annotation
  1. Filter variant calls based on INFO and/or FORMAT annotations ([`GATk VariantFiltration`](https://github.com/broadinstitute/gatk/releases))
  2. Variant annotation ([`SnpEff`](https://pcingola.github.io/SnpEff/snpeff/introduction/) and [`Annovar`](https://annovar.openbioinformatics.org/en/latest/))

7: GWAS
  1. Whole genome association analysis toolset ([`Plink`](https://www.cog-genomics.org/plink/))
  2. Genome-wide Complex Trait Analysis ([`gcta64`](https://yanglab.westlake.edu.cn/software/gcta/#Overview))
  3. Genome Association and Prediction Integrated Tool ([`GAPIT`](https://zzlab.net/GAPIT/))

