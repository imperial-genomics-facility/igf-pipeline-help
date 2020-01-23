---
layout: fastq_qc_template
title: IGF help pages - Fastq QC
---
# Information about Fastq QC Report
## Table of Contents

* [Overview](#overview)
* [List of genomes included for FastQ Screen analysis](#list-of-genomes-included-for-fastq-screen-analysis)
* [Software and version information](#software-and-version-information)
  * [FastQC command line](#fastqc-command-line)
  * [FastQ Screen command line](#fastq-screen-command-line)
* [List of resources](#list-of-resources)
* [Change logs](#change-logs)


## Overview

Raw data processing pipeline also generate basic quality reports for the fastq files using the following tools:

* [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc) for fastq reads quality check. See the official FastQC [help documents](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/) for more detail.
* [FastQ Screen](https://www.bioinformatics.babraham.ac.uk/projects/fastq_screen) for sample (species) contamination check
* [MultiQC](http://multiqc.info/) for merging all the reports (for all samples) together and for creating a top-level summary report of each lane (per project)

<div align="right"><a href="#table-of-contents">Go to Top</a></div>

## List of genomes included for FastQ Screen analysis

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

<div align="right"><a href="#table-of-contents">Go to Top</a></div>


## Software and version information

* [FastQC (v0.11.5)](http://www.bioinformatics.babraham.ac.uk/projects/fastqc)
* [FastQ screen (v0.11.1)](https://www.bioinformatics.babraham.ac.uk/projects/fastq_screen/)
* [MultiQC (v1.5)](http://multiqc.info/)

### FastQC command line
Example FastQC command.

<div style="background-color:#E8E8E8">
  <pre><code>
  fastqc 
    -q
    --noextract
    -f fastq 
    -k 7 
    -t 1
    -o /path/fastqc_output
    -d /path/temp_work_dir 
    /path/fastq_file
    
  </code></pre>
</div>

<div align="right"><a href="#table-of-contents">Go to Top</a></div>


### FastQ Screen command line
Example FastQ Screen command.

<div style="background-color:#E8E8E8">
  <pre><code>
  fastqscreen
    --conf fastqscreen_conf
    --outdir /path/fastqscreen_output
    --aligner bowtie2
    --force
    --quiet
    --subset 100000
    --threads 1
    /path/fastq_file
   
  </code></pre>
</div>

<div align="right"><a href="#table-of-contents">Go to Top</a></div>

## List of resources

* [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc)
* [FastQC help](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/)
* [Example of a good quality Illumina data (FastQC)](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html)
* [Example of a bad quality Illumina data (FastQC)](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html)
* [FastQ screen](https://www.bioinformatics.babraham.ac.uk/projects/fastq_screen/)
* [MultiQC](http://multiqc.info/)

<div align="right"><a href="#table-of-contents">Go to Top</a></div>

## Change logs

* 24th April 2019
  - Updated MultiQC from v 1.6 to v 1.7
* 2018 July 01
  - Updated MultiQC from v1.2
