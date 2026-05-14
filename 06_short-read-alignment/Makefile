# Accession number of the S.aureus genome.
ACC ?= CP009361

#The SRR number of the sequence data
SRR ?= SRR21835896

# The path to read 1
R1 ?= reads/${SRR}_1.fastq

# The path to read 2
R2 ?= reads/${SRR}_2.fastq

# The path to the reference genome
REF ?= refs/ref.fa

# The resulting BAM file.
BAM = bam/${SRR}.bam


usege:
	@echo ""
	@echo make all[download index align clear]
	@echo ""

all: download index align

download:
	mkdir -p refs reads
	fasterq-dump ${SRR} --split-files -O reads
	bio fetch --format fasta ${ACC} > refs/ref.fa

index:
	bwa index refs/ref.fa

align:
	mkdir -p bam
	bwa mem ${REF} ${R1} ${R2} | samtools sort > ${BAM}

clear:
	rm -rf refs reads bam