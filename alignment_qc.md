# Alignment QC

## Adapter trimming


### Fastp

https://github.com/OpenGene/fastp

<pre><code>
fastp
--in1 /path/input/sample.R1.fastq.gz
--out1 /path/trimmed/sample.R1.fastq.gz
--html /path/trimmed/sample.report.html
--json /path/trimmed/sample.report.json
--report_title sample
--thread 4
--in2 /path/input/sample.R2.fastq.gz
--out2 /path/trimmed/sample.R2.fastq.gz
--qualified_quality_phred=15
--length_required=15
--trim_poly_g [for NEXTSEQ]
</code></pre>

## RNA-Seq Alignment

### STAR

https://github.com/alexdobin/STAR

<pre><code>
STAR
--runThreadN 8
--outFileNamePrefix /path/mapped/sample
--outSAMattributes NH HI AS NM MD
--runMode alignReads
--outFilterType BySJout
--quantMode TranscriptomeSAM
--sjdbGTFfile gencode.v28.primary_assembly.annotation.gtf
--genomeLoad NoSharedMemory
--outSAMunmapped Within
--outSAMtype BAM SortedByCoordinate
--genomeDir GRCh38
--outFilterMultimapNmax 20
--alignIntronMin 20
--alignSJDBoverhangMin 1
--outFilterMismatchNoverReadLmax 0.04
--alignMatesGapMax 1000000
--limitBAMsortRAM 12000000000
--outFilterMismatchNmax 999
--alignIntronMax 1000000
--alignSJoverhangMin 8
--twopassMode Basic
--readFilesCommand zcat
--readFilesIn /path/trimmed/sample.R1.fastq.gz /path/trimmed/sample.R2.fastq.gz
</code></pre>


## DNA-Seq Alignment

### BWA

<pre><code>
bwa
mem
-t threads 
-M
/path/refgenome/fasta
/path/trimmed/sample.R1.fastq.gz /path/trimmed/sample.R2.fastq.gz
</code></pre>
## Post alignment processing

### Add RG tag

#### Picard RG tag

<pre><code>
/apps/java/jdk-8u144/bin/java
-XX:ParallelGCThreads=1
-Xmx4g
-Djava.io.tmpdir=/path/temp
-jar picard.jar
AddOrReplaceReadGroups
RGPL=ILLUMINA
RGPU=UNIQUE_RG_PU
RGLB=LIBRARY_ID
SORT_ORDER=unsorted
RGSM=SAMPLE_ID
RGCN=CENTER_NAME
RGID=UNIQUE_RG_ID
I=/path/mapped/sampleAligned.sortedByCoord.out.bam
O=/path/mapped/sampleAligned.sortedByCoord.out.AddOrReplaceReadGroups.bam
</code></pre>


### Mark duplicate reads

#### Picard Mark Duplicates

https://broadinstitute.github.io/picard/command-line-overview.html#MarkDuplicates

<pre><code>
/apps/java/jdk-8u144/bin/java
-XX:ParallelGCThreads=1
-Xmx4g
-Djava.io.tmpdir=/path/temp
-jar picard.jar, MarkDuplicates
O=/path/mapped/sample.genome.MarkDuplicates.bam
M=/path/mapped/sample.genome.MarkDuplicates.summary.txt
OPTICAL_DUPLICATE_PIXEL_DISTANCE=2500 [for HISEQ 4000 and NextSeq]
I=/path/mapped/sampleAligned.sortedByCoord.out.AddOrReplaceReadGroups.bam
</code></pre>



## RNA-Seq signal

### STAR bigwig

<pre><code>
STAR
--runThreadN 8
--runMode inputAlignmentsFromBAM
--outFileNamePrefix /path/signal/sample
--genomeLoad NoSharedMemory
--outWigType bedGraph
--outWigStrand Stranded
--inputBAMfile /path/mapped/sample.genome.MarkDuplicates.bam
</code></pre>

## RNA-Seq gene count

### FeatureCounts

<pre><code>
featureCounts
-a gencode.v28.primary_assembly.annotation.gtf
-o /path/output
-T 4
/path/mapped/sample.genome.MarkDuplicates.bam
</code></pre>

### RSEM

<pre><code>
rsem-calculate-expression
--quiet
--no-bam-output
--alignments
--strandedness reverse
--num-threads 8
--ci-memory 4000
--estimate-rspd
--paired-end 
/path/mapped/sample.merged.transcriptome.bam
RSEM_REF
/path/rsem/sample
</code></pre>

## MultiQC

A multiqc report for the alignment bam is produced (per sample) combining metrics from the following tools

* Fastp
* STAR (RNA-Seq)
* Picard Mark Duplicates
* Picard CollectAlignmentSummaryMetrics
* Picard CollectBaseDistributionByCycle
* Picard CollectGcBiasMetrics
* Picard QualityScoreDistribution
* Picard CollectRnaSeqMetrics (RNA-Seq)
* Samtools flagstat
* Samtools stats
* FeatureCounts (RNA-Seq)
