---
layout: sc_transcriptome_template
title: IGF help pages - Single Cell Transcriptome
---

# Single Cell Transcriptome Analysis (10xgenomics)
# Table of Contents

* [Overview](#overview)
* [Cellranger software and versions](#cellranger-software-and-versions)
  * [Cellranger command line](#cellranger-command-line)
* [Reference Genome](#reference-genome)
* [Information about cellranger run configuration](#information-about-cellranger-run-configuration)
* [Alignment summary metrics](#alignment-summary-metrics)
* [Singlecell QC check using Scanpy](#singlecell-qc-check-using-scanpy)
* [UCSC Cell Browser](#ucsc-cell-browser)
* [Software and versions](#software-and-versions)
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
    --disable-ui
    --localcores=14
    --localmem=64
    
  </code></pre>
</div>


## Reference Genome

Following reference genomes are used for running Cellranger data analysis pipeline:

* __Human__: A [custom reference genome](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/advanced/references) using the hg38 genome build and Gencode (v30) gene sets

* __Human (pre-mRNA)__: A [pre-mRNA reference genome](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/advanced/references) using the hg38 genome build and Gencode (v30) gene sets

## Information about cellranger run configuration
Check the __Sample__ information section of the Cellranger html report for more information regarding the reference genome build, Single cell chemistry version and Cellranger version information.


<p>
</p>
<div style="position:relative; left:50px">
  <img src="images/cellranger_sample_info.jpeg" height="150" style="border:1px solid black" >
</div>
<p>
</p>


## Alignment summary metrics

A multiqc report for the alignment bam is produced (per sample) combining the following Picard and Samtools metrics.

* __Picard CollectAlignmentSummaryMetrics__
* __Picard CollectBaseDistributionByCycle__
* __Picard CollectGcBiasMetrics__
* __Picard QualityScoreDistribution__
* __Picard CollectRnaSeqMetrics__
* __Samtools stats__
* __Samtools idxstats__

## Singlecell QC check using Scanpy

We use [Scanpy](https://scanpy.readthedocs.io/en/latest/) to generate per sample QC report for the single cell data following this tutorial: [Clustering 3K PBMCs](https://scanpy-tutorials.readthedocs.io/en/latest/pbmc3k.html). We have implemented a Jupyter notebook based QC report which can be run within a Docker or Singularity container. We execute this notebook based implementation for each of the single cell samples and store a html version of the report with all the codes and plots. These notebook resources can be accessed from the 'Analysis' tab of the 'IGF QC Report' page.

Please feel free to check our implementation of the QC report on Github [scanpy-notebook-image](https://github.com/imperial-genomics-facility/scanpy-notebook-image) for further detail or to try out the example notebooks on binder. A list of all our notebook based resources can be found this this page: [Notebook resources](notebook_resources.html).
 

## UCSC Cell Browser

We have implemented an offline version of [UCSC Cell Browser](https://cells.ucsc.edu/) and generate the server resources for individual samples. Similar to the Scanpy report pagess, these browser directories can be accessed from the 'Analysis' tab of the 'IGF QC Report' page.

## Software and versions

* [Scanpy v1.4.5](https://scanpy.readthedocs.io/en/latest/)
* [UCSC Cell Browser v 0.7.7](https://cells.ucsc.edu/)

## List of resources

* [Custom reference genome building](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/advanced/references)
* [Reference genome for hg19 and mm10](http://cf.10xgenomics.com/supp/cell-exp/refdata-cellranger-hg19-and-mm10-2.1.0.tar.gz)
* [Cellranger output files](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/output/overview)
* [Scanpy](https://scanpy.readthedocs.io/en/latest/)
* [UCSC Cell Browser](https://cells.ucsc.edu/)
* [Github: scanpy-notebook-image](https://github.com/imperial-genomics-facility/scanpy-notebook-image)
* [Notebook resources](notebook_resources.html)

## Change logs

* 3rd Feb 2020
  * Moved to notebook based Scanpy QC report
  * Updated Scanpy to v1.4 to v1.4.5
  * Added UCSC Cell Browser
* 25th June 2019
  * Added 3D UMAP plot
  * Added pre-mRNA reference transcriptome option for Cellranger
  * Moved to Gencode v30 build from v28
* 24th April 2019
  * Changed Cellranger version from 2.2.0 to 3.0.2
  * Changed Scanpy version from 1.2.2 to 1.4
  * Moved Scanpy command from help page and added them to the reports
* 24th August 2018
  * Added Scanpy report
