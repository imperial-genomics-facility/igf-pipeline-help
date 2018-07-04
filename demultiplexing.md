---
layout: demultiplexing_template
title: IGF Help pages - demultiplexing fastq
---
 
# Demultiplexing Fastq

## Overview

Illumina sequencing platforms generate binary BCL files for each run. These raw data files are picked up by genomic facility pipelines and processed for fastq file generation using software Bcl2Fastq (v 2.20). A samplesheet file containing correct index barcode information is essential for this "demultiplexing" process, for allocating fastq reads to the individual samples and filtering the artifacts present in the raw data.

## Software and version information

* [Bcl2Fastq (v2.20)](https://support.illumina.com/sequencing/sequencing_software/bcl2fastq-conversion-software/downloads.html)

## Samplesheet Format

Samplesheets are plain text, comma separated files. Its divided into multiple sections, which are marked by a line starting with a section label. Please check [Illumina documentation](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/sequencing-sheet-format-specifications-technical-note-970-2017-004.pdf) for more details about the samplesheet file format specification.


* Adapter information
* Sample name
* Index barcode information

## Demultiplexing of single cell samples (10xgenomics)

[List of single cell barcodes](https://support.10xgenomics.com/single-cell-gene-expression/sequencing/doc/specifications-sample-index-sets-for-single-cell-3)

## List of resources

* [Bcl2Fastq download page](https://support.illumina.com/sequencing/sequencing_software/bcl2fastq-conversion-software/downloads.html)
* [Bcl2Fastq documentation](https://support.illumina.com/content/dam/illumina-support/documents/documentation/software_documentation/bcl2fastq/bcl2fastq2_guide_15051736_v2.pdf)
* [Samplesheet Format Specifications](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/sequencing-sheet-format-specifications-technical-note-970-2017-004.pdf)
* [List of single cell barcodes](https://support.10xgenomics.com/single-cell-gene-expression/sequencing/doc/specifications-sample-index-sets-for-single-cell-3)

## Change logs

* None
