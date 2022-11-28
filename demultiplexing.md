---
layout: demultiplexing_template
title: IGF help pages - De-multiplexing Fastq
---


# De-multiplexing Fastq
## Table of Contents

* [De-multiplexing of Illumina sequencing runs](#de-multiplexing-of-illumina-sequencing-runs)
  * [Software versions](#software-versions)
* [De-multiplexing of Illumina sequencing runs using BCLConvert](#de-multiplexing-of-illumina-sequencing-runs-using-bclconvert)
  * [BCLConvert process summary](#bclconvert-process-summary)
  * [BCLConvert command line](#bclconvert-command-line)
  * [BCLConvert samplesheet format](#bclconvert-samplesheet-format)
  * [BCLConvert output directory structure](#bclconvert-output-directory-structure)
    * [BCLConvert fastq file validation](#bclconvert-fastq-file-validation)
* [De-multiplexing of Illumina sequencing runs using Bcl2Fastq](#de-multiplexing-of-illumina-sequencing-runs-using-bcl2fastq)
  * [Bcl2Fastq process summary](#bcl2fastq-process-summary)
  * [Bcl2Fastq command line](#bcl2fastq-command-line)
  * [Bcl2Fastq samplesheet format](#bcl2fastq-samplesheet-format)
    * [Bcl2Fastq adapter trimming setting](#bcl2fastq-adapter-trimming-setting)
    * [Bcl2Fastq fastq output](#bcl2fastq-fastq-output)
      * [Bcl2Fastq fastq file name](#bcl2fastq-fastq-file-name)
    * [Bcl2Fastq fastq file validation](#bcl2fastq-fastq-file-validation)
  * [Bcl2Fastq de-multiplexing of single cell samples (10xgenomics)](#bcl2fastq-de-multiplexing-of-single-cell-samples-10xgenomics)
    * [Bcl2Fastq command line for single cell samples](#bcl2fastq-command-line-for-single-cell-samples)
* [List of resources](#list-of-resources)


## De-multiplexing of Illumina sequencing runs

Illumina sequencing platforms generate binary BCL files for each run. These raw data files are picked up by genomic facility pipelines and processed for fastq file generation.
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

### Software versions

<div class="table-responsive">
  <table class="table table-hover">
    <thead style="font-weight:bold;">
      <tr class="table-light">
        <td scope="col">Instrument name</td>
        <td scope="col">Software name</td>
        <td scope="col">Software version</td>
        <td scope="col">Update date</td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>NextSeq 2000</td>
        <td>BCLConvert</td>
        <td>v3.9.3</td>
        <td>March 2022</td>
      </tr>
      <tr>
        <td>NovaSeq 6000</td>
        <td>BCLConvert</td>
        <td>v3.9.3</td>
        <td>May 2022</td>
      </tr>
      <tr>
        <td>MiSeq</td>
        <td>BCLConvert</td>
        <td>v3.9.3</td>
        <td>July 2022</td>
      </tr>
      <tr>
        <td>NovaSeq 6000</td>
        <td>Bcl2Fastq</td>
        <td>v2.20</td>
        <td>March 2021</td>
      </tr>
      <tr>
        <td>NextSeq 500</td>
        <td>Bcl2Fastq</td>
        <td>v2.20</td>
        <td>Feb 2018</td>
      </tr>
      <tr>
        <td>MiSeq</td>
        <td>Bcl2Fastq</td>
        <td>v2.20</td>
        <td>Feb 2018</td>
      </tr>
      <tr>
        <td>HiSeq 4000</td>
        <td>Bcl2Fastq</td>
        <td>v2.20</td>
        <td>Feb 2018</td>
      </tr>
      <tr>
        <td>HiSeq 4000</td>
        <td>Bcl2Fastq</td>
        <td>v2.18</td>
        <td>June 2017</td>
      </tr>
    </tbody>
  </table>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

## De-multiplexing of Illumina sequencing runs using BCLConvert

[BCLConvert](https://support.illumina.com/sequencing/sequencing_software/bcl-convert.html) tool is used for de-multiplexing data from the following sequening platforms

* Illumina NextSeq 2000
* Illumina NovaSeq 6000
* Illumina MiSeq
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

### BCLConvert process summary

Sequencing runs are processed using BCLConvert tool using the following steps:
* Generate __SampleSheet.csv__ file for the target sequencing run
* Replace sample barcode ids for any single cell sample (prepared using 10X Genomics library prep kit) with the actual index barcode sequence
* Group samples based on __Project name__, __Lane id__ (if available) and __Index barcode length__
* Edit __SampleSheet.csv__ file and add correct _OverrideCycles_ info for each of the sample group
* Add following __Settings__ in the __SampleSheet.csv__ file
  * _CreateFastqForIndexReads,1_
  * _MinimumTrimmedReadLength,8_
  * _FastqCompressionFormat,gzip_
  * _MaskShortReads,8_
  * _TrimUMI,0_ (Optional for samples with UMI barcodes)
* Configure and run BCLConvert tool for each sample group separately using the edited __SampleSheet.csv__ file
* Merge multiple fastq files to one set of fastqs, if there are more than one set of index barcodes present for any single cell sample (mostly for single index 10X Genomics samples)
* Generate HTML version of de-multiplexing report
* Generate FastQC, Fastq Screen and MultiQC reports


<div align="right"><a href="#table-of-contents">Go to Top</a></div>

### BCLConvert command line
<div style="background-color:#E8E8E8">
  <pre><code>  bcl-convert
    --bcl-input-directory /path/sequencing_run
    --output-directory /path/output
    --sample-sheet /path/SampleSheet.csv
    --bcl-num-conversion-threads 4
    --bcl-num-compression-threads 2
    --bcl-num-decompression-threads 2
    --bcl-num-parallel-tiles 4
    --bcl-sampleproject-subdirectories true
    --strict-mode true
    --bcl-only-lane lane_id
  </code></pre>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

### BCLConvert samplesheet format

SampleSheet file should have the following info:

* IGF de-multiplexing pipeline uses a [v1 format SampleSheet file](https://support-docs.illumina.com/SW/BCL_Convert/Content/SW/BCLConvert/SampleSheets_swBCL.htm) for the BCLConvert runs
* SampleSheet should contains following __Settings__ sections
  * __OverrideCycles,CYCLE_
  * _CreateFastqForIndexReads,1_
  * _MinimumTrimmedReadLength,8_
  * _FastqCompressionFormat,gzip_
  * _MaskShortReads,8_
  * _TrimUMI,0_ (Optional for samples with UMI barcodes)
* SampleSheet should contains the following __Data__ columns
  * _Lane_ (optional)
  * _Sample_ID_
  * _Sample_Name_
  * _Sample_Plate_
  * _Sample_Well_
  * _I7_Index_ID_
  * _index_
  * _I5_Index_ID_
  * _index2_
  * _Sample_Project_
  * _Description_
* No __Settings__ for _Adapter trimming_ gets added to SampleSheet by default (no adapter trimming by default)
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

### BCLConvert output directory structure
Fastq files from the BCLConvert run are organized using the following directory structure

`<ROOT DIR>/<SEQRUN DATE>/<FLOWCELL ID>/<LANE ID>/<INDEX GROUP>/<SAMPLE ID>/`

* __ROOT DIR__: It can be `PROJECT_NAME/fastq` or only `PROJECT_NAME`
* __SEQRUN DATE__: Sequencing run date or `PROJECT_NAME_FLOWCELL-ID_SEQRUN-DATE`
* __FLOWCELL ID__: Flowcell id from sequencing run
* __LANE ID__: Lane id information for fastq files
* __INDEX GROUP__: Sample barcode length and tag information
* __SAMPLE ID__: IGF sample id

Illumina uses the follwing file name convension for output fastq files. For example: __`sample-id_S1_L001_R1_001.fastq.gz`__

* __sample-id__ : Sample ID provided in the samplesheet
* __S1__ : Number of sample based on the sample order on the samplesheet
* __L001__ : Lane number of the flowcell
* __R1__ : The read. For e.g. R1 indicates Read 1 and R2 indicates Read 2 of a paired-end run
* __001__ : Its always 001
* __.fastq.gz__ : File extension. Its a gzipped fastq file

Each __SAMPLE ID__ level dir should contain the following fastq reads
  * __R1__ and __R2__ are the read 1 and read 2 (check more information about [Illumina paired-end](https://www.illumina.com/science/technology/next-generation-sequencing/paired-end-vs-single-read-sequencing.html) sequencing reads)
  * __I1__ and __I2__ are the index reads

Each __INDEX GROUP__ level dir can optionally contain these following files

* De-muliplexing html report dir (__`/Reports`__) with samplesheet file (for the lane)
* Manifest file (__`md5_manifest.tsv`__) containing the md5 checksum of the fastq files
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

#### BCLConvert fastq file validation
Steps:

* Download fastq files from Globus (if you are using Globus based transfer)
* In Mac/Linux
  * Open a terminal and `cd` to __INDEX GROUP__ level directory
  * Run md5sum for file validation, e.g.

  `grep -v file_path md5_manifest.tsv |md5sum -c`

  * Alternatively, use the following command for checking md5sum of fastqs file from all the flowcells
<div style="background-color:#E8E8E8">
  <pre><code>
  cd ROOT_DIR
  for i in `find . -name md5_manifest.tsv|xargs dirname`; \
    do \
      cd $i; \
      grep -v md5 md5_manifest.tsv |md5sum -c|grep -v OK; \
      cd -; \
    done
  </code></pre>
</div>

## De-multiplexing of Illumina sequencing runs using Bcl2Fastq
BCLConvert tool is used for de-multiplexing data from the following sequening platforms

* Illumina HiSeq 4000
* Illumina NextSeq 500
* Illumina NovaSeq 6000 (until May 2022)
* Illumina MiSeq (until May 2022)
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

### Bcl2Fastq process summary

Illumina sequencing platforms generate binary BCL files for each run. These raw data files are picked up by genomic facility pipelines and processed for fastq file generation using software BCLConvert. A samplesheet file containing correct index barcode information is essential for this "de-multiplexing" process, in order to allocate fastq reads to the individual samples and filtering the artifacts present in the raw data.
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

### Bcl2Fastq command line
Example Bcl2Fastq command.

<div style="background-color:#E8E8E8">
  <pre><code>  bcl2fastq
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

Additionally, for the short read cycles (less than 22 bp for R1 or R2), default value for bcl2fastq param **--mask-short-adapter-reads** is modified with the length of the read cycle.
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

### Bcl2Fastq samplesheet format

Samplesheets are plain text files, separated by commas, with name __`SampleSheet.csv`__. It is divided into multiple sections, which are marked by a line starting with a section label. Please check [Illumina documentation](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/sequencing-sheet-format-specifications-technical-note-970-2017-004.pdf) for more details about the samplesheet file format specification.

* SampleSheet should contains the following __Data__ columns
  * _Lane_ (optional)
  * _Sample_ID_
  * _Sample_Name_
  * _Sample_Plate_
  * _Sample_Well_
  * _I7_Index_ID_
  * _index_
  * _I5_Index_ID_
  * _index2_
  * _Sample_Project_
  * _Description_
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

#### Bcl2Fastq adapter trimming setting

De-multiplexing pipeline is configured to trim Illumina generic adapters from the reads, with the default run settings.

<div class="table-responsive">
  <table class="table table-hover">
    <thead style="font-weight:bold;">
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
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

#### Bcl2Fastq fastq output

Fastq files can be accessed from our iRODS data distribution server. Please check the [Data access](data_access.html) page for more details on this topic. Following files are present in each of the lane level tar files

* Fastq reads
  - __R1__ and __R2__ are the read 1 and read 2 (check more information about [Illumina paired-end](https://www.illumina.com/science/technology/next-generation-sequencing/paired-end-vs-single-read-sequencing.html) sequencing reads)
  - __I1__ and __I2__ are the index reads
* Demuliplexing html report
* Manifest file containing the md5 checksum of the fastq files
* Samplesheet file (for the lane)
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

##### Bcl2Fastq fastq file name

Illumina uses the follwing file name convension for the output fastq files

For example: __`sample-name_S1_L001_R1_001.fastq.gz`__

* __sample-name__ : Name of the sample provided in the samplesheet
* __S1__ : Number of sample based on the sample order on the samplesheet
* __L001__ : Lane number of the flowcell
* __R1__ : The read. For e.g. R1 indicates Read 1 and R2 indicates Read 2 of a paired-end run
* __001__ : Its always 001
* __.fastq.gz__ : File extension. Its a gzipped fastq file

Please check the [Illumina BCL2Fastq documentation](https://emea.support.illumina.com/sequencing/sequencing_software/bcl2fastq-conversion-software.html) for more information.
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

#### Bcl2Fastq fastq file validation

Steps:

* Download tar files from iRODS server and extract (use 7zip for windows)
* In Mac/Linux
  * Open a terminal and `cd` to the top level dir (look for `PROJECT_NAME_file_manifest.csv`)
  * Run md5sum for file validation 
  
  e.g 
  
  ```
  awk '{print $2"  " $1}' PROJECT_NAME_file_manifest.csv |grep -v file|md5sum -c
  ```
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

### Bcl2Fastq de-multiplexing of single cell samples (10xgenomics)

De-multiplexing of single cell samples are done using the specific set of [single cell barcodes](https://support.10xgenomics.com/single-cell-gene-expression/sequencing/doc/specifications-sample-index-sets-for-single-cell-3) following the 10xgenomics's [documentation](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/bcl2fastq-direct).

#### Bcl2Fastq command line for single cell samples
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
<div align="right"><a href="#table-of-contents">Go to Top</a></div>

## List of resources

* [BCLConvert](https://support.illumina.com/sequencing/sequencing_software/bcl-convert.html)
* [Bcl2Fastq download page](https://support.illumina.com/sequencing/sequencing_software/bcl2fastq-conversion-software/downloads.html)
* [Bcl2Fastq documentation](https://support.illumina.com/content/dam/illumina-support/documents/documentation/software_documentation/bcl2fastq/bcl2fastq2_guide_15051736_v2.pdf)
* [Samplesheet Format Specifications](https://www.illumina.com/content/dam/illumina-marketing/documents/products/technotes/sequencing-sheet-format-specifications-technical-note-970-2017-004.pdf)
* [List of single cell barcodes](https://support.10xgenomics.com/single-cell-gene-expression/sequencing/doc/specifications-sample-index-sets-for-single-cell-3)
* [Samplesheet validation schema](https://github.com/imperial-genomics-facility/data-management-python/tree/master/data/validation_schema#samplesheet-validation)

<div align="right"><a href="#table-of-contents">Go to Top</a></div>
