# Accession number of the S.aureus genome.
ACC ?= NC_002549.1

#The SRR number of the sequence data
SRR ?= SRR1972739

# The path to read 1
R1 ?= reads/${SRR}_1.fastq

# The path to read 2
R2 ?= reads/${SRR}_2.fastq

# The path to the reference genome
REF ?= refs/ref.fa

# The resulting BAM file.
BAM = bam/${SRR}.bam

# The temporary bedgraph file
BG=bam/${SRR}.bedgraph

# The BW wiggle file
BW=bam/${SRR}.bw


usege:
	@echo ""
	@echo make all[download index align clear]
	@echo ""

all: download index align

download:
	mkdir -p refs reads annotation
	fasterq-dump ${SRR} --split-files -O reads
	bio fetch --format fasta ${ACC} > refs/ref.fa
	bio fetch --format gff ${ACC} > annotation/ann.gff 


index:
	bwa index refs/ref.fa
	samtools faidx ${REF}

align:
	mkdir -p bam
	bwa mem ${REF} ${R1} ${R2} | samtools sort > ${BAM}
	samtools index ${BAM}
	LC_ALL=C; bedtools genomecov -ibam  ${BAM} -split -bg | sort -k1,1 -k2,2n > ${BG}
	bedGraphToBigWig ${BG} ${REF}.fai ${BW}

clear:
	rm -rf refs reads bam annotation