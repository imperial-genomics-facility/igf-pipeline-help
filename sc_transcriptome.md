---
layout: sc_transcriptome_template
title: IGF Help Pages - Single Cell Transcriptome
---
* [Single Cell Transcriptome Analysis (10xgenomics)](# Single Cell Transcriptome Analysis (10xgenomics))
  * [Overview](## Overview)
  * [Software and versions](## Software and versions)
   * [Cellranger command line](### Cellranger command line) 

# Single Cell Transcriptome Analysis (10xgenomics)

## Overview

[Cellranger]((https://support.10xgenomics.com/single-cell-gene-expression/software/downloads/latest)) pipeline from 10Xgenomics is used for running primary analysis for the single cell transcriptome samples. A list of the output files from this pipeline can be found [here](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/output/overview).

## Software and versions

* [Cellranger (2.1.1)](https://support.10xgenomics.com/single-cell-gene-expression/software/downloads/latest)

### Cellranger command line
Example cellranger count command.

<pre><code>
  cellranger count 
    --fastqs=/fastq_directory_path 
    --id=ID 
    --transcriptome=/path/transcriptome
    --nopreflight 
    --maxjobs=10 
    --mempercore=4 
    --disable-ui 
    --jobmode=pbspro 
    --localcores=1 
    --localmem=4
    
</code></pre>


## Reference Genome

Following reference genomes are used for running Cellranger data analysis pipeline:

* Human: A [custom reference genome](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/advanced/references) using the hg38 genome build and Gencode (v28) gene sets
* Human and mouse mixture:  A mixed genome of hg19 and mm10 from [Cellranger website](http://cf.10xgenomics.com/supp/cell-exp/refdata-cellranger-hg19-and-mm10-2.1.0.tar.gz)
* Mouse: mm10 reference genome from [Cellranger website](http://cf.10xgenomics.com/supp/cell-exp/refdata-cellranger-mm10-2.1.0.tar.gz)


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

 * [Clustering 3k PBMCs following a Seurat Tutorial](https://nbviewer.jupyter.org/github/theislab/scanpy_usage/blob/master/170505_seurat/seurat.ipynb)
 * [Profiling Scanpy for 68k PBMC cells of Zheng et al., Nat. Comm. (2017)](https://nbviewer.jupyter.org/github/theislab/scanpy_usage/blob/master/170503_zheng17/zheng17.ipynb)

### Software and versions

* [Scanpy (1.2.2)](https://scanpy.readthedocs.io/en/latest/)

### Scanpy command line

<pre><code>
# step 1: read cellranger output matrix
adata=sc.read(self.matrix_file,
              cache=True).T
adata.var_names = pd.read_csv(gene_tsv,header=None,
                              sep='\t')[1]
adata.obs_names = pd.read_csv(barcode_tsv,header=None)[0]
adata.var_names_make_unique()


# step 2: filter data based on cell and genes
sc.pp.filter_cells(adata,min_genes=self.min_gene_count)
sc.pp.filter_genes(adata,min_cells=self.min_cell_count)


# step 3: fetch mitochondrial genes
mt_genes=sc.queries.mitochondrial_genes(url,species_name)

## filter mito genes which are not present in data
mt_genes=[name for name in adata.var_names if name in mt_genes]           


# step 4: calculate mitochondrial read percentage
adata.obs['percent_mito']=np.sum(adata[:, mt_genes].X, axis=1).A1 / np.sum(adata.X, axis=1).A1

## add the total counts per cell as observations-annotation to adata
adata.obs['n_counts']=adata.X.sum(axis=1).A1

## violin plot of the computed quality measures                   
sc.pl.violin(adata,['n_genes', 'n_counts', 'percent_mito'],jitter=0.4,multi_panel=True,show=True,save='.png')                                                 

## scatter plots for data quality    
sc.pl.scatter(adata,x='n_counts',y='percent_mito',show=True,save='.png')                                                    
sc.pl.scatter(adata,x='n_counts',y='n_genes',save='.png')                                                


# step 5: Filtering data bases on percent mito
adata=adata[adata.obs['n_genes']<2000,:]
adata=adata[adata.obs['percent_mito'] < 0.05, :]

## logarithmize data and copy raw data
adata.raw=sc.pp.log1p(adata, copy=True)                                   


# step 6: Normalise and filter data
sc.pp.normalize_per_cell(adata)

## identify highly variable genes
filter_result=sc.pp.filter_genes_dispersion(adata.X,flavor='seurat')

## plot variable genes            
sc.pl.filter_genes_dispersion(filter_result,show=True,save='.png')                                

## replace data with filtered data
adata=adata[:, filter_result.gene_subset]                                 


# step 7: Analyze data

## logarithmize the data
sc.pp.log1p(adata)

## regress out effects of total counts per cell and the percentage of mitochondrial genes expressed                                                        
sc.pp.regress_out(adata, ['n_counts', 'percent_mito'])  

## scale the data to unit variance                  
sc.pp.scale(adata, max_value=10)

## run PCA                                       
sc.tl.pca(adata)

## plot pca loading graph                                                          
sc.pl.pca_loadings(adata,show=True,save='.png')                                           

## run tSNE
sc.tl.tsne(adata,random_state=2,n_pcs=10)                       

## generate neighborhood graph                               
sc.pp.neighbors(adata, n_neighbors=10)

## run louvain graph clustering                                  
sc.tl.louvain(adata)

## plot tSNE data                                                  
sc.pl.tsne(adata,color='louvain',show=True,save='.png')                                                   


# step 8: Finding marker genes
sc.tl.rank_genes_groups(adata, 'louvain')

## plot rank gene
sc.pl.rank_genes_groups(adata,n_genes=20,show=True,save='.png')                                      

result = adata.uns['rank_genes_groups']
groups = result['names'].dtype.names

## generate table data for rank gene
gene_score=pd.DataFrame({group + '_' + key: result[key][group]
                          for group in groups 
                            for key in ['names', 'scores']})               

## violin plot of marker genes (group 0)
group=result['names'].dtype.names:
key='{0}_names'.format(group)

## collect name of top 5 variable genes
genes=gene_score[key].head(5).to_dict().values()

## plot data
sc.pl.violin(adata,genes,groupby='louvain',show=True,save='.png',multi_panel=False,
             scale='width',multi_panel_figsize=[8.0,16.0])     </code></pre>

## List of resources

* [Custom reference genome building](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/advanced/references)
* [Reference genome for hg19 and mm10](http://cf.10xgenomics.com/supp/cell-exp/refdata-cellranger-hg19-and-mm10-2.1.0.tar.gz)
* [Cellranger mm10 reference genome](http://cf.10xgenomics.com/supp/cell-exp/refdata-cellranger-mm10-2.1.0.tar.gz)
* [Cellranger output files](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/output/overview)
* [Scanpy](https://scanpy.readthedocs.io/en/latest/)

## Change logs

* None
