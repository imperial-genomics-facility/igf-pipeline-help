---
layout: analysis_template
title: IGF help pages - Analysis pipelines
---

<h1>Data Analysis Pipelines</h1>

We now support running lots of community provided and vendor supplied pipelines for projects which are sequenced by us.


<div class="table-responsive">
  <table class="table table-hover" id="table-of-contents">
    <thead style="font-weight:bold;">
      <tr class="table-light">
        <td scope="col">Analyses name</td>
        <td scope="col">Pipeline used</td>
        <td scope="col">Status</td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><a href="#faq">FAQ</a></td>
        <td></td>
        <td></td>
      </tr>
      <tr>
        <td><a href="#snakemake-workflows_rna-seq-star-deseq2">RNA-Seq alignment and DE</a></td>
        <td><a href="https://github.com/snakemake-workflows/rna-seq-star-deseq2">snakemake-workflows/rna-seq-star-deseq2</a></td>
        <td>Active</td>
      </tr>
      <tr>
        <td><a href="#nf-core_rnaseq">RNA-Seq alignment and QC</a></td>
        <td><a href="https://nf-co.re/rnaseq">nf-core/rnaseq</a></td>
        <td>Active</td>
      </tr>
      <tr>
        <td><a href="#cellranger">Single cell RNA-Seq data analysis for 10X genomics library</a></td>
        <td><a href="https://www.10xgenomics.com/support/software/cell-ranger">Cellranger</a></td>
        <td>Active</td>
      </tr>
      <tr>
        <td><a href="#cellranger-arc">Single cell Gene expression and ATAC-Seq multiome data analysis for 10X genomics library</a></td>
        <td><a href="https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/overview/welcomer">Cellranger-ARC</a></td>
        <td>Active</td>
      </tr>
      <tr>
        <td><a href="#nf-core_atacseq">ATAC-Seq alignment, peak calling and QC</a></td>
        <td><a href="https://nf-co.re/atacseq">nf-core/atacseq</a></td>
        <td>Active</td>
      </tr>
      <tr>
        <td><a href="#geomxngspipeline">Count data (DCC) and QC for GeoMx Digital Spatial Profiler (DSP) system</a></td>
        <td><a href="https://nanostring.com/products/geomx-digital-spatial-profiler/geomx-dsp-overview/">geomxngspipeline</a></td>
        <td>Active</td>
      </tr>
      <tr>
        <td><a href="#nf-core_sarek">WGS or Exome variant calling</a></td>
        <td><a href="https://nf-co.re/sarek">nf-core/sarek</a></td>
        <td>Active</td>
      </tr>
      <tr>
        <td><a href="#nf-core_smrnaseq">Small RNA-Seq alignment and QC</a></td>
        <td><a href="https://nf-co.re/smrnaseq">nf-core/smrnaseq</a></td>
        <td>Active</td>
      </tr>
      <tr>
        <td><a href="#nf-core_methylseq">Bisulfite-Sequencing alignment and QC</a></td>
        <td><a href="https://nf-co.re/methylseq">nf-core/methylseq</a></td>
        <td>Active</td>
      </tr>
      <tr>
        <td>ChIP-Seq alignment, peak calling and QC</td>
        <td><a href="#nf-core_chipseq">nf-core/chipseq</a></td>
        <td>Untested</td>
      </tr>
      <tr>
        <td>Phylogeny analysis from bacterial whole genome sequences</td>
        <td><a href="#nf-core_bactmap">nf-core/bactmap</a></td>
        <td>Untested</td>
      </tr>
      <tr>
        <td>CUT&TAG and CUT&RUN alignment and QC</td>
        <td><a href="#nf-core_cutandrun">nf-core/cutandrun</a></td>
        <td>Untested</td>
      </tr>
      <tr>
        <td>Hi-C data alignment and QC</td>
        <td><a href="#nf-core_hic">nf-core/hic</a></td>
        <td>Untested</td>
      </tr>
      <tr>
        <td>Amplicon sequencing analysis workflow using DADA2 and QIIME2</td>
        <td><a href="#nf-core_ampliseq">nf-core/ampliseq</a></td>
        <td>Untested</td>
      </tr>
    </tbody>
  </table>
</div>
<p/>
<div>
  <h2 id="faq">FAQ</h2>
  <table class="table" style="border:hidden;">
    <tbody>
      <tr>
        <td style="border:hidden; width:50%"><b>Can we get a custom reference genome for our project?</b></td>
        <td style="border:hidden;">Yes, we can build custom reference genome for collaborative projects.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:50%"><b>How do you transfer data?</b></td>
        <td style="border:hidden;">We add a new <code>analysis</code> directory to the Globus collection for analysis files. Files are available only for <b>30 days</b>.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:50%"><b>Will you run these pipelines automatically?</b></td>
        <td style="border:hidden;">No, we run them for any specific project based on users requests.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:50%"><b>How you run NF-core pipelines on HPC?</b></td>
        <td style="border:hidden;">We use a custom configuration file to run NF-core workflows on Imperial College's HPC and track runs via a local installation of <a href="https://github.com/seqeralabs/nf-tower">Nextflow Tower (community edition)</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:50%"><b>Can we access your Nextflow Tower server?</b></td>
        <td style="border:hidden;">We are open for discussions.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:50%"><b>How do you manage multiple pipeline runs on HPC?</b></td>
        <td style="border:hidden;">We use <a href="https://airflow.apache.org/">Apache Airflow</a> as orchestration manager and queue multiple pipelines with correct <code>queue</code> and <code>pool</code> information.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:50%"><b>Can you run these pipelines for externally sequenced fastq files?</b></td>
        <td style="border:hidden;"><b>NO.</b> We can run them only for the projects which are sequenced by us. Internally, we organise our files following <a href="https://ena-docs.readthedocs.io/en/latest/submit/general-guide/metadata.html">ENA metadata model</a> which helps us to configure these pipelines programmatically. But you can ask for our help if you are trying to run these pipelines on externally sequenced data.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:50%"><b>Can you add XYZ pipeline to this list?</b></td>
        <td style="border:hidden;">Yes. We are continuously adding new pipelines to the above list. Feel free to suggest us if your project needs any new analysis pipeline.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:50%"><b>Why some of these pipelines are marked Untested?</b></td>
        <td style="border:hidden;">We have checked them only with test data but haven't used them for any real project.</td>
      </tr>
    </tbody>
  </table>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>
<div>
  <h2 id="snakemake-workflows_rna-seq-star-deseq2">Snakemake RNA-Seq workflow</h2>
  <table class="table" style="border:hidden;">
    <tbody>
      <tr>
        <td style="border:hidden; width:25%"><b>Pipeline name:</b></td>
        <td style="border:hidden;"><a href="https://github.com/snakemake-workflows/rna-seq-star-deseq2">snakemake-workflows/rna-seq-star-deseq2</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Supported versions:</b></td>
        <td style="border:hidden;">Latest (>= 2.0.0)</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Default reference genome:</b></td>
        <td style="border:hidden;"><a href="https://www.ensembl.org/info/about/species.html">Ensembl species</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Required inputs:</b></td>
        <td style="border:hidden;">
        <p>We need following details to configure and run this pipeline:</p>
          <ul>
            <li>List of sample IGF ids</li>
            <li>Reference genome to use from Ensembl(e.g. Homo sapiens)</li>
            <li>Sample groups for DESeq2 analysis</li>
          </ul>
        <details>
          <summary>Click here for more information</summary><p/>
          <p><b>Reference genome</b></p>
            <ul>
              <li>Species name (e.g., homo sapiens)</li>
              <li>Ensembl release number (e.g., 110), for using any specific version of annotation</li>
              <li>Genome build tag (e.g., GRCh38)</li>
            </ul>
          <p><b>Sample metadata</b></p>
          Simple metadata:<pre style="background-color:#E8E8E8">  sample_id,condition
  IGF001,untreated
  IGF002,treated</pre>
    Complex metadata:<pre style="background-color:#E8E8E8">  sample_id,treatment_1,treatment_2
  IGF001,untreated,untreated
  IGF002,untreated,treated
  IGF003,untreated,treated</pre>
          <p><b>Sample group info</b></p>
          Simple group: Check this <a href="https://github.com/snakemake-workflows/rna-seq-star-deseq2/blob/master/.test/config_basic/config.yaml">example</a>. For more information on simple contrast check <a href="https://www.bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html#contrasts">this page</a>.
          <pre style="background-color:#E8E8E8">  Group: treated-vs-untreated
      variable_of_interest: condition
      level_of_interest: treated</pre>
          Complex group: Check this <a href="https://github.com/snakemake-workflows/rna-seq-star-deseq2/blob/master/.test/config_complex/config.yaml">example</a>. For more information about complex contrast specification, check <a href="https://github.com/tavareshugo/tutorial_DESeq2_contrasts/blob/main/DESeq2_contrasts.md">this page</a>.
          <pre style="background-color:#E8E8E8">  Group: treatment_1_alone
    variable_of_interest: treatment_1
    Slevel_of_interest: treated</pre>
        </details></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Pipeline description:</b></td>
        <td style="border:hidden;">
        <ul>
        <li>Trim (if configured) and align individual fastq groups (i.e., sample + flowcell lane) using STAR</li>
        <li>Combine gene counts using a custom tools, i.e., combine counts from <code>results/star/{unit.sample_name}_{unit.unit_name}/ReadsPerGene.out.tab</code> files and merge them to <code>results/counts/all.tsv</code></li>
        <li>Create normalized gene counts via DESeq2 uning this <a href="https://github.com/snakemake-workflows/rna-seq-star-deseq2/blob/master/workflow/scripts/deseq2-init.R">R script</a> and output RDS file as well.</li>
        <li>Compare individial groups vis DESeq2 using this <a href="https://github.com/snakemake-workflows/rna-seq-star-deseq2/blob/master/workflow/scripts/deseq2.R">R script</a>.</li>
        </ul>
        </td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Output results:</b></td>
        <td style="border:hidden;">
        <ul>
        <li>STAR aligned BAM files</li>
        <li>Combined gene counts for all samples in <code>.tsv</code> format</li>
        <li>MultiQC report</li>
        <li>DESeq2 output files and PCA plots (based on analysis configuration)</li>
        </ul>
        </td>
      </tr>
    </tbody>
  </table>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>
<div>
  <h2 id="nf-core_rnaseq">NF-core RNA-Seq workflow</h2>
  <table class="table" style="border:hidden;">
    <thead>
    </thead>
    <tbody>
      <tr>
        <td style="border:hidden; width:25%"><b>Pipeline name:</b></td>
        <td style="border:hidden;"><a href="https://nf-co.re/rnaseq">nf-co.re/rnaseq</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Supported versions:</b></td>
        <td style="border:hidden;">Latest (>= 3.12.0)</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Default reference genome:</b></td>
        <td style="border:hidden;"><a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a>. Please note that the default iGenome annotations are <a href="https://nf-co.re/docs/usage/reference_genomes">currently out-of-date</a>. E.g., human iGenomes annotations are from Ensembl release 75 (<a href="http://feb2014.archive.ensembl.org/index.html">February 2014</a>). Check out our <a href="#faq">FAQ</a> section for more information about custom reference genomes.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Required inputs:</b></td>
        <td style="border:hidden;">
          <p>We need following details to configure and run this pipeline:</p>
            <ul>
              <li>List of sample IGF ids and replicate information</li>
              <li>Reference genome from <a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a>(e.g. GRCh38)</li>
              <li>List of <a href="https://nf-co.re/rnaseq/3.12.0/parameters/">parameters</a> for NF-core pipeline run</li>
            </ul>
          <details>
            <summary>Click here for more information</summary><p/>
              <p><b>List of sample IGF ids</b>. For e.g.,</p>
                <pre style="background-color:#E8E8E8">  IGF001
  IGF002</pre>
              <p><b>List of NF-core RNA-Seq pipeline parameters</b>. For e.g.,</p>
               <pre style="background-color:#E8E8E8">  --aligner star_rsem
  --deseq2_vst</pre>
          </details>
        </td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Output results:</b></td>
        <td style="border:hidden;"><a href="https://nf-co.re/rnaseq/latest/docs/output">NF-core RNA-Seq output</a></td>
      </tr>
    </tbody>
  </table>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>
<div>
  <h2 id="cellranger">Single cell RNA-Seq data analysis for 10X genomics library</h2>
  <table class="table" style="border:hidden;">
    <thead>
    </thead>
    <tbody>
      <tr>
        <td style="border:hidden; width:25%"><b>Pipeline name:</b></td>
        <td style="border:hidden;"><a href="https://www.10xgenomics.com/support/software/cell-ranger">Cellranger</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Supported versions:</b></td>
        <td style="border:hidden;">Latest (>= 7.2)</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Default reference genome:</b></td>
        <td style="border:hidden;">Reference genomes from <a href="https://www.10xgenomics.com/support/software/cell-ranger/downloads">10X genomics</a>.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Required inputs:</b></td>
        <td style="border:hidden;">
          <p>We need following details to configure and run this pipeline:</p>
            <ul>
              <li>List of sample IGF ids and their feature types, i.e. Gene Expression, VDJ-B or VDJ-T</li>
              <li>Cellranger multi group information</li>
              <li>Reference genome information</li>
            </ul>
          <details>
            <summary>Click here for more information</summary><p/>
              <p><b>List of sample IGF ids and feature types</b>. For e.g.,</p>
                <pre style="background-color:#E8E8E8">  #igf_id,feature_types,cellranger_group
  IGF001,Gene Expression,Group1
  IGF002,VDJ-B,Group1
  IGF003,Gene Expression,Group2
  IGF004,VDJ-T,Group2
  IGF005,Gene Expression,Group3</pre>
          </details>
        </td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Output results:</b></td>
        <td style="border:hidden;">
          <ul>
            <li><a href="https://www.10xgenomics.com/support/software/cell-ranger/analysis/running-pipelines/cr-3p-multi">Cellranger multi output for individual groups</a></li>
            <li><a href="https://www.10xgenomics.com/support/software/cell-ranger/analysis/outputs/cr-outputs-aggr-outputs">Output of Cellranger Aggr</a></li>
            <li><a href="https://github.com/imperial-genomics-facility/scanpy-notebook-image/tree/master/templates">Scanpy QC report for individual and aggregated samples</a></li>
          </ul>
        </td>
      </tr>
    </tbody>
  </table>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>
<div>
  <h2 id="cellranger-arc">Single cell Gene expression and ATAC-Seq multiome data analysis for 10X genomics library</h2>
  <table class="table" style="border:hidden;">
    <thead>
    </thead>
    <tbody>
      <tr>
        <td style="border:hidden; width:25%"><b>Pipeline name:</b></td>
        <td style="border:hidden;"><a href="https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/overview/welcome">Cellranger-ARC</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Supported versions:</b></td>
        <td style="border:hidden;">Latest (>= 2.0.2)</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Default reference genome:</b></td>
        <td style="border:hidden;">Reference genomes from <a href="https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/downloads/latest">10X genomics</a>.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Required inputs:</b></td>
        <td style="border:hidden;">
          <p>We need following details to configure and run this pipeline:</p>
            <ul>
              <li>List of sample IGF ids and their library type, i.e. Gene Expression or Chromatin Accessibility</li>
              <li>Cellranger-ARC group information</li>
              <li>Reference genome information</li>
            </ul>
          <details>
            <summary>Click here for more information</summary><p/>
              <p><b>List of sample IGF ids and library type</b>. For e.g.,</p>
                <pre style="background-color:#E8E8E8">  #igf_id,feature_types,cellranger_group
  IGF001,Gene Expression,Group1
  IGF002,Chromatin Accessibility,Group1
  IGF003,Gene Expression,Group2
  IGF004,Chromatin Accessibility,Group2</pre>
          </details>
        </td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Output results:</b></td>
        <td style="border:hidden;">
          <ul>
            <li><a href="https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/pipelines/latest/output/overview">Cellranger-ARC output for individual groups</a></li>
            <li><a href="https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/pipelines/latest/using/aggr">Output of Cellranger-ARC Aggr</a></li>
            <li><a href="https://github.com/imperial-genomics-facility/scanpy-notebook-image/tree/master/templates">Scanpy QC report for individual and aggregated samples</a></li>
          </ul>
        </td>
      </tr>
    </tbody>
  </table>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>
<div>
  <h2 id="nf-core_atacseq">NF-core ATAC-Seq workflow</h2>
  <table class="table" style="border:hidden;">
    <thead>
    </thead>
    <tbody>
      <tr>
        <td style="border:hidden; width:25%"><b>Pipeline name:</b></td>
        <td style="border:hidden;"><a href="https://nf-co.re/atacseq">nf-co.re/atacseq</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Supported versions:</b></td>
        <td style="border:hidden;">Latest (>= 2.1.2)</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Default reference genome:</b></td>
        <td style="border:hidden;"><a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a>. Please note that the default iGenome annotations are <a href="https://nf-co.re/docs/usage/reference_genomes">currently out-of-date</a>. E.g., human iGenomes annotations are from Ensembl release 75 (<a href="http://feb2014.archive.ensembl.org/index.html">February 2014</a>). Check out our <a href="#faq">FAQ</a> section for more information about custom reference genomes.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Required inputs:</b></td>
        <td style="border:hidden;">
          <p>We need following details to configure and run this pipeline:</p>
            <ul>
              <li>List of sample IGF ids</li>
              <li>Control samples for peak calling (if any)</li>
              <li>Reference genome from <a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a>(e.g. GRCh38)</li>
              <li>List of <a href="https://nf-co.re/atacseq/2.1.2/parameters/">parameters</a> for NF-core pipeline run</li>
            </ul>
          <details>
            <summary>Click here for more information</summary><p/>
              <p><b>List of sample IGF ids and replicates</b>. For e.g.,</p>
                <pre style="background-color:#E8E8E8">  #igf_id,sample_group,replicate_id
  IGF001,CONTROL,1
  IGF002,CONTROL,2
  IGF003,CONTROL,3</pre>
              <p><b>Peak calling control samples (if any)</b>. For e.g.,</p>
                <pre style="background-color:#E8E8E8">  #igf_id,sample_group,replicate_id,control,control_replicate
  IGF001,CONTROL,1,,
  IGF002,CONTROL,2,,
  IGF003,CONTROL,3,,
  IGF004,TREATMENT,1,CONTROL,1
  IGF005,TREATMENT,2,CONTROL,2
  IGF006,TREATMENT,3,CONTROL,3</pre>
              <p><b>List of NF-core ATAC-Seq pipeline parameters</b>. For e.g.,</p>
                <pre style="background-color:#E8E8E8">  --trim_nextseq 20
  --aligner bwa
  --narrow_peak</pre>
          </details>
        </td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Output results:</b></td>
        <td style="border:hidden;"><a href="https://nf-co.re/atacseq/latest/docs/output">NF-core ATAC-Seq output</a></td>
      </tr>
    </tbody>
  </table>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>
<div>
  <h2 id="geomxngspipeline">GeoMx NGS Pipeline</h2>
  <table class="table" style="border:hidden;">
    <thead>
    </thead>
    <tbody>
      <tr>
        <td style="border:hidden; width:25%"><b>Pipeline name:</b></td>
        <td style="border:hidden;"><a href="https://nanostring.com/products/geomx-digital-spatial-profiler/geomx-dsp-overview/">GeoMx NGS Pipeline</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Supported versions:</b></td>
        <td style="border:hidden;">Latest (>= 2.3.3.10)</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Required inputs:</b></td>
        <td style="border:hidden;">
          <p>We need following details to configure and run this pipeline:</p>
            <ul>
              <li>List of sample IGF ids and DSP ids</li>
              <li>GeoMx experiment configuration file</li>
              <li>Probe assay metadata describing the gene targets present in the data, PKC files can be found <a href="https://nanostring.com/products/geomx-digital-spatial-profiler/geomx-dsp-configuration-files/">here</a></li>
            </ul>
          <details>
            <summary>Click here for more information</summary><p/>
              <p><b>List of sample IGF ids and DSP ids</b>. For e.g.,</p>
                <pre style="background-color:#E8E8E8">  #igf_id,dsp_id
  IGF001,DSP001
  IGF002,DSP002</pre>
          </details>
        </td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Output results:</b></td>
        <td style="border:hidden;">
          <ul>
            <li>DCC count files</li>
            <li>DCC count QC based on latest version of these <a href="https://github.com/imperial-genomics-facility/igf-dockerfiles/tree/main/bioconductor-geomxworkflows/templates">templates</a></li>
          </ul>
        </td>
      </tr>
    </tbody>
  </table>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>
<div>
  <h2 id="nf-core_sarek">NF-core Sarek workflow</h2>
  <table class="table" style="border:hidden;">
    <thead>
    </thead>
    <tbody>
      <tr>
        <td style="border:hidden; width:25%"><b>Pipeline name:</b></td>
        <td style="border:hidden;"><a href="https://nf-co.re/sarek">nf-co.re/sarek</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Supported versions:</b></td>
        <td style="border:hidden;">Latest (>= 3.4.0)</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Default reference genome:</b></td>
        <td style="border:hidden;"><a href="https://ewels.github.io/AWS-iGenomes/">iGenome - GATK.GRCh38</a>. Please note that the default iGenome annotations are <a href="https://nf-co.re/docs/usage/reference_genomes">currently out-of-date</a>. E.g., human iGenomes annotations are from Ensembl release 75 (<a href="http://feb2014.archive.ensembl.org/index.html">February 2014</a>). Check out our <a href="#faq">FAQ</a> section for more information about custom reference genomes.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Required inputs:</b></td>
        <td style="border:hidden;">
          <p>We need following details to configure and run this pipeline:</p>
            <ul>
              <li>List of sample IGF ids</li>
              <li><b>Patient ID:</b> Custom patient ID for normal and tumor samples</li>
              <li><b>Sex:</b> Sex chromosomes of the patient; i.e. XX, XY or NA, only used for Copy-Number Variation analysis in a tumor/pair</li>
              <li><b>Status:</b> Normal/tumor status of sample; can be 0 (normal) or 1 (tumor)</li>
              <li>Reference genome from <a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a>(e.g. GRCh38)</li>
              <li>List of <a href="https://nf-co.re/sarek/3.4.0/parameters">parameters</a> for NF-core pipeline run</li>
            </ul>
          <details>
            <summary>Click here for more information</summary><p/>
              <p><b>List of sample IGF ids and replicates</b>. For e.g.,</p>
                <pre style="background-color:#E8E8E8">  #igf_id,patient,sex,status
  IGF001,ID1,XX,0
  IGF002,ID2,XY,0
  IGF003,ID3,XX,0</pre>
              <p><b>List of NF-core Sarek pipeline parameters</b>. For e.g.,</p>
               <pre style="background-color:#E8E8E8">  --genome GATK.GRCh38
  --wes
  --intervals path_to_intervals_file
  --save_mapped
  --concatenate_vcfs
  --tools haplotypecaller,snpeff,vep</pre>
          </details>
        </td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Output results:</b></td>
        <td style="border:hidden;"><a href="https://nf-co.re/sarek/latest/docs/output">NF-core Sarek output</a></td>
      </tr>
    </tbody>
  </table>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>
<div>
  <h2 id="nf-core_smrnaseq">NF-core smRNASeq workflow</h2>
  <table class="table" style="border:hidden;">
    <thead>
    </thead>
    <tbody>
      <tr>
        <td style="border:hidden; width:25%"><b>Pipeline name:</b></td>
        <td style="border:hidden;"><a href="https://nf-co.re/smrnaseq">nf-co.re/smrnaseq</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Supported versions:</b></td>
        <td style="border:hidden;">Latest (>= 2.2.4)</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Default reference genome:</b></td>
        <td style="border:hidden;"><a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a>. Please note that the default iGenome annotations are <a href="https://nf-co.re/docs/usage/reference_genomes">currently out-of-date</a>. E.g., human iGenomes annotations are from Ensembl release 75 (<a href="http://feb2014.archive.ensembl.org/index.html">February 2014</a>). Check out our <a href="#faq">FAQ</a> section for more information about custom reference genomes.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Required inputs:</b></td>
        <td style="border:hidden;">
          <p>We need following details to configure and run this pipeline:</p>
            <ul>
              <li>List of sample IGF ids</li>
              <li><a href="https://nf-co.re/smrnaseq/latest/docs/usage">Experimental protocol</a></li>
              <li>Reference genome from <a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a>(e.g. GRCh38)</li>
              <li>List of <a href="https://nf-co.re/smrnaseq/2.2.4/parameters">parameters</a> for NF-core pipeline run</li>
            </ul>
          <details>
            <summary>Click here for more information</summary><p/>
              <p><b>List of sample IGF ids</b>. For e.g.,</p>
                <pre style="background-color:#E8E8E8">  #igf_id
  IGF001
  IGF002
  IGF003</pre>
              <p><b>List of NF-core smRNAseq pipeline parameters</b>. For e.g.,</p>
               <pre style="background-color:#E8E8E8">  --genome GRCh38</pre>
          </details>
        </td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Output results:</b></td>
        <td style="border:hidden;"><a href="https://nf-co.re/smrnaseq/latest/docs/output">NF-core smRNAseq output</a></td>
      </tr>
    </tbody>
  </table>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>
<div>
  <h2 id="nf-core_methylseq">NF-core Methylseq workflow</h2>
  <table class="table" style="border:hidden;">
    <thead>
    </thead>
    <tbody>
      <tr>
        <td style="border:hidden; width:25%"><b>Pipeline name:</b></td>
        <td style="border:hidden;"><a href="https://nf-co.re/methylseq">nf-co.re/methylseq</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Supported versions:</b></td>
        <td style="border:hidden;">Latest (>= 2.5.0)</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Default reference genome:</b></td>
        <td style="border:hidden;"><a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a>. Please note that the default iGenome annotations are <a href="https://nf-co.re/docs/usage/reference_genomes">currently out-of-date</a>. E.g., human iGenomes annotations are from Ensembl release 75 (<a href="http://feb2014.archive.ensembl.org/index.html">February 2014</a>). Check out our <a href="#faq">FAQ</a> section for more information about custom reference genomes.</td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Required inputs:</b></td>
        <td style="border:hidden;">
          <p>We need following details to configure and run this pipeline:</p>
            <ul>
              <li>List of sample IGF ids</li>
              <li>Reference genome from <a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a>(e.g. GRCh38)</li>
              <li>List of <a href="https://nf-co.re/methylseq/latest/parameters">parameters</a> for NF-core pipeline run</li>
            </ul>
          <details>
            <summary>Click here for more information</summary><p/>
              <p><b>List of sample IGF ids</b>. For e.g.,</p>
                <pre style="background-color:#E8E8E8">  #igf_id
  IGF001
  IGF002
  IGF003</pre>
              <p><b>List of NF-core methylseq pipeline parameters</b>. For e.g.,</p>
               <pre style="background-color:#E8E8E8">  --genome GRCh38</pre>
          </details>
        </td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Output results:</b></td>
        <td style="border:hidden;"><a href="https://nf-co.re/methylseq/latest/docs/output">NF-core methylseq output</a></td>
      </tr>
    </tbody>
  </table>
</div>
<div align="right"><a href="#table-of-contents">Go to Top</a></div>
