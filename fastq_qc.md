---
layout: fastq_qc_template
title: IGF Help Pages - Fastq QC
---
    

# Information about Fastq QC Report

## Overview

Raw data processing pipeline also generate basic quality reports for the fastq files using the following tools:

* [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc) for Fastq reads quality check
* [FastQ Screen](https://www.bioinformatics.babraham.ac.uk/projects/fastq_screen) for Sample species contamination check
* [MultiQC](http://multiqc.info/) for merging all the reports (for all samples) together and for creating a top-level summary report of each lane (per project)

### List of genomes included for FastQ Screen analysis

* Human (HG38)
* Mouse (mm10)
* Ecoli (Escherichia_coli_K_12_MG1655)
* PhiX
* Staphylococcus 
* Adapters ([FastQC](www.bioinformatics.babraham.ac.uk/projects/fastqc) contaminats file)
* Mycobacterium (Mycobacterium_tuberculosis_H37RV)
* Vectors ([UniVec](http://www.ncbi.nlm.nih.gov/VecScreen/UniVec.html))
* Mycoplasma (GCF_000027345.1_ASM2734v1)
* Rhodococcus
* Paenibacillus


## Software and version information

* [FastQC (v0.11.5)](http://www.bioinformatics.babraham.ac.uk/projects/fastqc)
* [FastQ screen (v0.11.1)](https://www.bioinformatics.babraham.ac.uk/projects/fastq_screen/)
* [MultiQC (v1.5)](http://multiqc.info/)

## List of resources

* [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc)
* [FastQC help](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/)
* [Example of a good quality Illumina data (FastQC)](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html)
* [Example of a bad quality Illumina data (FastQC)](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html)
* [FastQ screen](https://www.bioinformatics.babraham.ac.uk/projects/fastq_screen/)
* [MultiQC](http://multiqc.info/)

## Change logs

* 2018 July 01
  - Updated MultiQC from v1.2
