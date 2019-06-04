---
layout: sc_transcriptome_template
title: IGF Help Pages - Single Cell Transcriptome
---

# Single Cell Transcriptome Analysis (10xgenomics)
# Table of Contents

* [Overview](#overview)
* [Cellranger software and versions](#cellranger-software-and-versions)
  * [Cellranger command line](#cellranger-command-line)
* [Reference Genome](#reference-genome)
* [Alignment summary metrics](#alignment-summary-metrics)
* [Singlecell QC check using Scanpy](#singlecell-qc-check-using-scanpy)
  * [Scanpy software and versions](#scanpy-software-and-versions)
* [List of resources](#list-of-resources)
* [Change logs](#change-logs)
  
## Overview

[Cellranger]((https://support.10xgenomics.com/single-cell-gene-expression/software/downloads/latest)) pipeline from 10Xgenomics is used for running primary analysis for the single cell transcriptome samples. A list of the output files from this pipeline can be found [here](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/output/overview).

## Cellranger software and versions

* [Cellranger (3.0.2)](https://support.10xgenomics.com/single-cell-gene-expression/software/downloads/latest)

### Cellranger command line
Example cellranger count command.

<div style="background-color:#E8E8E8">
  <pre><code>
  cellranger count
    --fastqs=/fastq_directory_path
    --id=ID
    --transcriptome=/path/transcriptome
    --nopreflight
    --maxjobs=20
    --mempercore=4
    --disable-ui
    --jobmode=pbspro
    --localcores=1
    --localmem=4
    
  </code></pre>
</div>


## Reference Genome

Following reference genomes are used for running Cellranger data analysis pipeline:

* Human: A [custom reference genome](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/advanced/references) using the hg38 genome build and Gencode (v28) gene sets


## Alignment summary metrics

A multiqc report for the alignment bam is produced (per sample) combining the following Picard and Samtools metrics.

* Picard CollectAlignmentSummaryMetrics
* Picard CollectBaseDistributionByCycle
* Picard CollectGcBiasMetrics
* Picard QualityScoreDistribution
* Picard CollectRnaSeqMetrics
* Samtools flagstat
* Samtools idxstats

## Singlecell QC check using Scanpy

Scanpy tool is used for checking an initial qc of the single cell data after cellranger count run. This analysis is based on the following Scanpy tutorials

  * [Clustering 3K PBMCs](https://scanpy-tutorials.readthedocs.io/en/latest/pbmc3k.html)


### Scanpy software and versions

* [Scanpy (1.4)](https://scanpy.readthedocs.io/en/latest/)

## List of resources

* [Custom reference genome building](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/advanced/references)
* [Reference genome for hg19 and mm10](http://cf.10xgenomics.com/supp/cell-exp/refdata-cellranger-hg19-and-mm10-2.1.0.tar.gz)
* [Cellranger output files](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/output/overview)
* [Scanpy](https://scanpy.readthedocs.io/en/latest/)

## Change logs

* 24th April 2019
  * Changed Cellranger version from 2.2.0 to 3.0.2
  * Changed Scanpy version from 1.2.2 to 1.4
  * Moved Scanpy command from help page and added them to the reports
* 24th August 2018
  * Added Scanpy report
