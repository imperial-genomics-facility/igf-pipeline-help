---
layout: analysis_template
title: IGF help pages - Data Access
---

<h1>Data Analysis Pipelines</h1>

TODO: Intro

<!--

<div class="table-responsive">
  <table class="table table-hover">
    <thead style="font-weight:bold;">
      <tr class="table-light">
        <td scope="col">Analyses name</td>
        <td scope="col">Pipeline used</td>
        <td scope="col">Status</td>
      </tr>
    </thead>
    <tbody>
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
        <td>ChIP-Seq alignment, peak calling and QC</td>
        <td><a href="#nf-core_chipseq">nf-core/chipseq</a></td>
        <td>Untested</td>
      </tr>
      <tr>
        <td>WGS or Exome variant calling</td>
        <td><a href="#nf-core_sarek">nf-core/sarek</a></td>
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
        <td>Small RNA-Seq alignment and QC</td>
        <td><a href="#nf-core_smrnaseq">nf-core/smrnaseq</a></td>
        <td>Active</td>
      </tr>
      <tr>
        <td>Bisulfite-Sequencing alignment and QC</td>
        <td><a href="#nf-core_methylseq">nf-core/methylseq</a></td>
        <td>Active</td>
      </tr>
      <tr>
        <td>Amplicon sequencing analysis workflow using DADA2 and QIIME2</td>
        <td><a href="#nf-core_ampliseq">nf-core/ampliseq</a></td>
        <td>Untested</td>
      </tr>
      
    </tbody>
  </table>
</div>
-->
<h2 id="snakemake-workflows_rna-seq-star-deseq2">Snakemake RNA-Seq workflow</h2>

<div>
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
        <p>
          We need following details to configure and run this pipeline:
          <ul>
            <li>List of sample IGF ids</li>
            <li>Reference genome to use from Ensembl(e.g. Homo sapiens)</li>
            <li>Sample groups for DESeq2 analysis</li>
          </ul>
        </p>
        <details>
          <summary>Click here for more information</summary><p/>
          <p><b>Reference genome</b></p>
            <ul>
              <li>Species name (e.g., homo sapiens)</li>
              <li>Ensembl release number (e.g., 110), for using any specific version of annotation</li>
              <li>Genome build tag (e.g., GRCh38)</li>
            </ul>
          <p><b>Sample metadata</b></p>
          Simple metadata:<pre>sample_id,condition
    IGF001,untreated
    IGF002,treated</pre>
    Complex metadata:<pre>
  sample_id,treatment_1,treatment_2
  IGF001,untreated,untreated
  IGF002,untreated,treated
  IGF003,untreated,treated</pre>
          <p><b>Sample group info</b></p>
          Simple group:
          Check this <a href="https://github.com/snakemake-workflows/rna-seq-star-deseq2/blob/master/.test/config_basic/config.yaml">example</a>
          <pre><code>
  Group: treated-vs-untreated
    variable_of_interest: condition
    level_of_interest: treated
          </pre></code>
          Complex group:
          Check this <a href="https://github.com/snakemake-workflows/rna-seq-star-deseq2/blob/master/.test/config_complex/config.yaml">example</a>
          <pre>Group: treatment_1_alone
    variable_of_interest: treatment_1
    level_of_interest: treated</pre>
        </details></td>
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

<!--
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
        <td style="border:hidden;"><a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Required inputs:</b></td>
        <td style="border:hidden;">
          <p>
            We need following details to configure and run this pipeline:
            <ul>
              <li>List of sample IGF ids and replicate information</li>
              <li>Reference genome from <a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a>(e.g. GRCh38)</li>
              <li>List of <a href="https://nf-co.re/rnaseq/3.12.0/parameters/">parameters</a> for NF-core pipeline run</li>
            </ul>
          </p>
          <details>
            <summary>Click here for more information</summary><p/>
              <p>
                <b>List of sample IGF ids</b>. For e.g.,
                <pre><code>
                IGF001
                IGF002
                </code></pre>
              </p>
              <p>
               <b>List of NF-core RNA-Seq pipeline parameters</b>. For e.g.,
               <pre><code>
                 --aligner star_rsem
                 --deseq2_vst
               </pre></code>
              </p>
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
        <td style="border:hidden;"><a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a></td>
      </tr>
      <tr>
        <td style="border:hidden; width:25%"><b>Required inputs:</b></td>
        <td style="border:hidden;">
          <p>
            We need following details to configure and run this pipeline:
            <ul>
              <li>List of sample IGF ids</li>
              <li>Control samples for peak calling (if any)</li>
              <li>Reference genome from <a href="https://ewels.github.io/AWS-iGenomes/">iGenome</a>(e.g. GRCh38)</li>
              <li>List of <a href="https://nf-co.re/atacseq/2.1.2/parameters/">parameters</a> for NF-core pipeline run</li>
            </ul>
          </p>
          <details>
            <summary>Click here for more information</summary><p/>
              <p>
                <b>List of sample IGF ids and replicates</b>. For e.g.,
                <pre><code>
                #igf_id,sample_group,replicate_id
                IGF001,CONTROL,1
                IGF002,CONTROL,2
                IGF003,CONTROL,3
                </code></pre>
              </p>
              <p>
                <b>Peak calling control samples (if any)</b>. For e.g.,
                <pre><code>
                #igf_id,sample_group,replicate_id,control,control_replicate
                IGF001,CONTROL,1,,
                IGF002,CONTROL,2,,
                IGF003,CONTROL,3,,
                IGF004,TREATMENT,1,CONTROL,1
                IGF005,TREATMENT,2,CONTROL,2
                IGF006,TREATMENT,3,CONTROL,3
                </code></pre>
              </p>
              <p>
               <b>List of NF-core ATAC-Seq pipeline parameters</b>. For e.g.,
               <pre><code>
                 --trim_nextseq 20
                 --aligner bwa
                 --narrow_peak
               </pre></code>
              </p>
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
          <p>
            We need following details to configure and run this pipeline:
            <ul>
              <li>List of sample IGF ids and DSP ids</li>
              <li>GeoMx experiment configuration file</li>
              <li>Probe assay metadata describing the gene targets present in the data, PKC files can be found <a href="https://nanostring.com/products/geomx-digital-spatial-profiler/geomx-dsp-configuration-files/">here</a></li>
            </ul>
          </p>
          <details>
            <summary>Click here for more information</summary><p/>
              <p>
                <b>List of sample IGF ids and DSP ids</b>. For e.g.,
                <pre><code>
                #igf_id,dsp_id
                IGF001,DSP001
                IGF002,DSP002
                </code></pre>
              </p>
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
-->
<!--


<h2 id="nf-core_chipseq">NF-core ChIP-Seq workflow</h2>

<h2 id="nf-core_sarek">NF-core Sarek</h2>



<h2 id="nf-core_bactmap">NF-core Bacterial phylogeny analysis</h2>

<h2 id="nf-core_cutandrun">NF-core CUT&RUN anslysis</h2>

<h2 id="nf-core_hic">NF-core Hi-C data anslysis</h2>

<h2 id="nf-core_smrnaseq">NF-core Small RNA-Seq analysis</h2>

<h2 id="nf-core_methylseq">NF-core Methylation (Bisulfite-Sequencing) analysis</h2>

<h2 id="nf-core_ampliseq">NF-core Amplicon sequencing analysis</h2>
-->