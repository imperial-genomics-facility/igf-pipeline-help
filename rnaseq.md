Fastp

https://github.com/OpenGene/fastp

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


STAR

https://github.com/alexdobin/STAR

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




Picard RG tag


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
I=/path/mapped/sampleAligned.toTranscriptome.out.bam
O=/path/mapped/sampleAligned.toTranscriptome.out.AddOrReplaceReadGroups.bam


Mark Duplicates

https://broadinstitute.github.io/picard/command-line-overview.html#MarkDuplicates

/apps/java/jdk-8u144/bin/java
-XX:ParallelGCThreads=1
-Xmx4g
-Djava.io.tmpdir=/path/temp
-jar picard.jar, MarkDuplicates
O=/path/mapped/sample.genome.MarkDuplicates.bam
M=/path/mapped/sample.genome.MarkDuplicates.summary.txt
OPTICAL_DUPLICATE_PIXEL_DISTANCE=2500 [for HISEQ 4000 and NextSeq]
I=/path/mapped/sampleAligned.sortedByCoord.out.AddOrReplaceReadGroups.bam


Samtools merge

/project/tgu/software/samtools/samtools, merge
--output-fmt BAM
--threads 4
-b /path/mapped/sample.bam_chunk.txt
-n /path/mapped/sample.merged.transcriptome.bam



STAR bigwig

STAR
--runThreadN 8
--runMode inputAlignmentsFromBAM
--outFileNamePrefix /path/signal/sample
--genomeLoad NoSharedMemory
--outWigType bedGraph
--outWigStrand Stranded
--inputBAMfile /path/mapped/sample.genome.MarkDuplicates.bam


RSEM

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
