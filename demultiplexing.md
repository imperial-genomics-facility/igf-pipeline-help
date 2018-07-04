---
layout: demultiplexing_template
title: IGF Help pages - demultiplexing fastq
---
 
# Demultiplexing Fastq

## Overview

Illumina sequencing platforms generate binary BCL files for each run. These raw data files are picked up by genomic facility pipelines and processed for fastq file generation using software Bcl2Fastq. A samplesheet file containing correct index barcode information is essential for this "demultiplexing" process, for allocating fastq reads to the individual samples and filtering the artifacts present in the raw data.

## Software and version information

* [Bcl2Fastq (v2.20)](https://support.illumina.com/sequencing/sequencing_software/bcl2fastq-conversion-software/downloads.html)

## Samplesheet Format

Samplesheets are plain text, comma separated files with name `SampleSheet.csv`. Its divided into multiple sections, which are marked by a line starting with a section label. Please check [Illumina documentation](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/sequencing-sheet-format-specifications-technical-note-970-2017-004.pdf) for more details about the samplesheet file format specification.

### Data Format

Following are the required columns for `[Data]` section of the samplesheet files. A validation schema for this samplesheet section can be found [here](https://github.com/imperial-genomics-facility/data-management-python/tree/master/data/validation_schema#samplesheet-validation).


  <table class="table table-hover">
    <thead>
      <tr class="table-light">
        <td scope="header">Column Name</td>
        <td scope="header">Allowed characters</td>
        <td scope="header">Comment</td>
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
        <td>An unique id will be assiged by genomics facility for each samples</td>
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





### Adapter trimming

Demultiplexing pipeline is configured to trim Illumina generic adapters from the reads, with the default run settings.


| Adapter name | Adapter Sequence                  |
|--------------|-----------------------------------|
| Adapter      | AGATCGGAAGAGCACACGTCTGAACTCCAGTCA |
| AdapterRead2 | AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT |



## Demultiplexing of single cell samples (10xgenomics)

[List of single cell barcodes](https://support.10xgenomics.com/single-cell-gene-expression/sequencing/doc/specifications-sample-index-sets-for-single-cell-3)

## List of resources

* [Bcl2Fastq download page](https://support.illumina.com/sequencing/sequencing_software/bcl2fastq-conversion-software/downloads.html)
* [Bcl2Fastq documentation](https://support.illumina.com/content/dam/illumina-support/documents/documentation/software_documentation/bcl2fastq/bcl2fastq2_guide_15051736_v2.pdf)
* [Samplesheet Format Specifications](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/sequencing-sheet-format-specifications-technical-note-970-2017-004.pdf)
* [List of single cell barcodes](https://support.10xgenomics.com/single-cell-gene-expression/sequencing/doc/specifications-sample-index-sets-for-single-cell-3)
* [Samplesheet validation schema](https://github.com/imperial-genomics-facility/data-management-python/tree/master/data/validation_schema#samplesheet-validation)

## Change logs

* None
