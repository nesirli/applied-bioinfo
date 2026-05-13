# Write a Bash script

### Add commands to download at least one sequencing dataset using the SRR number(s).
```bash
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR218/096/SRR21835896/SRR21835896_1.fastq.gz
```
### Download only a subset of the data that would provide approximately 10x genome coverage. Briefly explain how you estimated the amount of data needed for 10x coverage.
```text
The Staphylococcus aureus genome is typically about 2.8 Megabases (Mb) (or 2.8 million base pairs) in length, with a range generally between 2.5 and 2.9 Mbp.
For 10x coverage: based needed = 10 * genome size = 10 * 2.800.000 = 28.000.000 bp
reads needed = 28.000.000 bp / 101 bp/read = 277.228
Approximately 280.000 reads are needed to cover S.aureus genome 10 times.
```
```bash
fasterq-dump -X 280000 SRR21835896
```

# Quality assessment

### Generate basic statistics on the downloaded reads (e.g., number of reads, total bases, average read length).
```bash
seqkit stats SRR21835896_1.fastq.gz 
```
```text
file                    format  type    num_seqs        sum_len  min_len  avg_len  max_len
SRR21835896_1.fastq.gz  FASTQ   DNA   15,754,542  1,591,208,742      101      101      101
```

### Run FASTQC on the downloaded data to generate a quality report.
```bash
fastqc SRR21835896_1.fastq.gz
```
### Evaluate the FASTQC report and summarize your findings.
```text
Basic Statistics	✅ PASS	Consistent, high-quality dataset
Per base sequence quality	✅ PASS	Excellent quality across all positions
Per sequence quality scores	✅ PASS	Reads have high average quality
Per base N content	✅ PASS	No ambiguous bases detected
Sequence Length Distribution	✅ PASS	Uniform 101 bp length
Adapter Content	✅ PASS	No adapter contamination
Per sequence GC content	⚠️ WARNING	Slight deviation from expected normal distribution
Per base sequence content	❌ FAIL	Nucleotide bias in early positions
Sequence Duplication Levels	❌ FAIL	High sequence duplication
Overrepresented sequences	❌ FAIL	Specific sequences highly repetitive
```

### Perform any necessary quality control steps (e.g., trimming, filtering) and briefly describe your process.
```bash
fastp -i SRR21835896_1.fastq.gz \
      -o SRR21835896_1.trimmed.fastq.gz \
      --cut_front 5 \
      --trim_poly_g \
      -q 20 \
      -h fastp_report.html
```
```text
--cut_front 5 — Fixes sequence bias
--trim_poly_g — Removes polyG tails (actual Illumina artifacts)
-q 20 — Phred quality score (the data is already high quality)
```


# Compare sequencing platforms

### Search the SRA for another dataset for the same genome, but generated using a different sequencing platform (e.g., if original data was Illumina select PacBio or Oxford Nanopore).
```bash
fasterq-dump SRR38477249
```
### Briefly compare the quality or characteristics of the datasets from the two platforms.
```bash
seqkit stats SRR38477249.fastq
```
```text
file               format  type  num_seqs        sum_len  min_len   avg_len  max_len
SRR38477249.fastq  FASTQ   DNA    500,715  7,362,091,741      131  14,703.2   43,655
```
