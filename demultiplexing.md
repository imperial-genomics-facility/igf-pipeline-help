---
layout: demultiplexing_template
title: IGF Help pages - demultiplexing fastq
---
 
# Demultiplexing Fastq
## Table of Contents

* [Overview](#overview)
* [Software and version information](#software-and-version-information)
  * [Bcl2Fastq command line](#bcl2fastq-command-line)
* [Samplesheet Format](#samplesheet-format)
  * [Data Format](#data-format)
  * [Adapter trimming setting](#adapter-trimming-setting)
* [Fastq output](#fastq-output)
* [Demultiplexing of single cell samples (10xgenomics)](#demultiplexing-of-single-cell-samples-10xgenomics)
  * [Bcl2Fastq command line for single cell samples](#bcl2fastq-command-line-for-single-cell-samples)
* [List of resources](#list-of-resources)
* [Change logs](#change-logs)


## Overview

Illumina sequencing platforms generate binary BCL files for each run. These raw data files are picked up by genomic facility pipelines and processed for fastq file generation using software Bcl2Fastq. A samplesheet file containing correct index barcode information is essential for this "demultiplexing" process, in order to allocate fastq reads to the individual samples and filtering the artifacts present in the raw data.

## Software and version information

* [Bcl2Fastq (v2.20)](https://support.illumina.com/sequencing/sequencing_software/bcl2fastq-conversion-software/downloads.html)

### Bcl2Fastq command line
Example Bcl2Fastq command.

<div style="background-color:#E8E8E8">
  <pre><code>
  bcl2fastq 
    --runfolder-dir /path/input 
    --sample-sheet /path/SampleSheet.csv 
    --output-dir /path/output 
    --reports-dir /path/Reports 
    --use-bases-mask BASES_MASK 
    --stats-dir /path/Stats 
    -p 2 
    --create-fastq-for-index-reads 
    -w 1 
    --barcode-mismatches 1 
    -r 1 
    --auto-set-to-zero-barcode-mismatches
  
  </code></pre>
</div>

## Samplesheet Format

Samplesheets are plain text files, separated by commas, with name `SampleSheet.csv`. It is divided into multiple sections, which are marked by a line starting with a section label. Please check [Illumina documentation](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/sequencing-sheet-format-specifications-technical-note-970-2017-004.pdf) for more details about the samplesheet file format specification.

### Data Format

Following are the required columns for `[Data]` section of the samplesheet files. A validation schema for this samplesheet section can be found [here](https://github.com/imperial-genomics-facility/data-management-python/tree/master/data/validation_schema#samplesheet-validation).

<div class="table-responsive">
  <table class="table table-hover">
    <thead>
      <tr class="table-light">
        <td scope="col">Column Name</td>
        <td scope="col">Allowed characters</td>
        <td scope="col">Comment</td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Lane</td>
        <td>Number 1 to 8</td>
        <td>Optional, only required for Hiseq runs</td>
      </tr>
      <tr>
        <td>Sample_ID</td>
        <td>A-Z, 0-9, "-" (NO SPACE)</td>
        <td>An unique id will be assigned by genomics facility for each samples</td>
      </tr>
      <tr>
        <td>Sample_Name</td>
        <td>A-Z, 0-9, "-"   (NO SPACE)</td>
        <td>User given sample name which will be used by Bcl2Fastq for naming fastq files</td>
      </tr>
      <tr>
        <td>Sample_Project</td>
        <td>A-Z, 0-9, "-"   (NO SPACE)</td>
        <td>Name of the project, with quotation id</td>
      </tr>
      <tr>
        <td>I7_Index_ID</td>
        <td>alphanumeric</td>
        <td>Required</td>
      </tr>
      <tr>
        <td>index</td>
        <td>A string of "ATGC" or "SI-GA-[A-Z][digits]"</td>
        <td>Required</td>
      </tr>
      <tr>
        <td>I5_Index_ID</td>
        <td>alphanumeric</td>
        <td>Optional, only required for dual index runs</td>
      </tr>
      <tr>
        <td>index2</td>
        <td>A string of ATGC</td>
        <td>Optional</td>
      </tr>
      <tr>
        <td>Sample_Plate</td>
        <td>alphanumeric</td>
        <td>Optional</td>
      </tr>
      <tr>
        <td>Sample_Well</td>
        <td>alphanumeric</td>
        <td>Optional</td>
      </tr>
      <tr>
        <td>Description</td>
        <td>alphanumeric</td>
        <td>Only accepted value is "10X" for single cell samples</td>
      </tr>
    </tbody>
  </table>
</div>



### Adapter trimming setting

Demultiplexing pipeline is configured to trim Illumina generic adapters from the reads, with the default run settings.


<div class="table-responsive">
  <table class="table table-hover">
    <thead>
      <tr class="table-light">
        <td scope="col">Adapter name</td>
        <td scope="col">Adapter Sequence</td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Adapter</td>
        <td>AGATCGGAAGAGCACACGTCTGAACTCCAGTCA</td>
      </tr>
      <tr>
        <td>AdapterRead2</td>
        <td>AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT</td>
      </tr>
       </tbody>
  </table>
</div>


## Fastq output

Fastq files can be accessed from our iRODS data distribution server. Please check the [Data access](data_access.html) page for more details on this topic. Following files are present in each of the lane level tar files

* Fastq reads
  - R1 and R2 are the read 1 and read 2 (check more information about [Illumina paired-end](https://www.illumina.com/science/technology/next-generation-sequencing/paired-end-vs-single-read-sequencing.html) sequencing reads)
  - I1 and I2 are the index reads
* Demuliplexing html report
* Manifest file containing the md5 checksum of the fastq files
* Samplesheet file (for the lane)


## Demultiplexing of single cell samples (10xgenomics)

Demultiplexing of single cell samples are done using the specific set of [single cell barcodes](https://support.10xgenomics.com/single-cell-gene-expression/sequencing/doc/specifications-sample-index-sets-for-single-cell-3) following the 10xgenomics's [documentation](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/bcl2fastq-direct).

### Bcl2Fastq command line for single cell samples
Example Bcl2Fastq command.

<div style="background-color:#E8E8E8">
  <pre><code>
  bcl2fastq 
    --runfolder-dir /path/input 
    --sample-sheet /path/SampleSheet.csv 
    --output-dir /path/output 
    --reports-dir /path/Reports 
    --use-bases-mask BASES_MASK 
    --stats-dir /path/Stats 
    -p 2 
    --create-fastq-for-index-reads 
    -w 1 
    --barcode-mismatches 1 
    -r 1 
    --auto-set-to-zero-barcode-mismatches
    --mask-short-adapter-reads=8
    --minimum-trimmed-read-length=8
  
  </code></pre>
</div>

## List of resources

* [Bcl2Fastq download page](https://support.illumina.com/sequencing/sequencing_software/bcl2fastq-conversion-software/downloads.html)
* [Bcl2Fastq documentation](https://support.illumina.com/content/dam/illumina-support/documents/documentation/software_documentation/bcl2fastq/bcl2fastq2_guide_15051736_v2.pdf)
* [Samplesheet Format Specifications](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/sequencing-sheet-format-specifications-technical-note-970-2017-004.pdf)
* [List of single cell barcodes](https://support.10xgenomics.com/single-cell-gene-expression/sequencing/doc/specifications-sample-index-sets-for-single-cell-3)
* [Samplesheet validation schema](https://github.com/imperial-genomics-facility/data-management-python/tree/master/data/validation_schema#samplesheet-validation)

## Change logs

* 2018 Feb 25
  - Updated Bcl2fastq tool from version v2.18
