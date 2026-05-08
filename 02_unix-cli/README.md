### Download the file
```bash
wget https://ftp.ensembl.org/pub/current_gff3/felis_catus/Felis_catus.F.catus_Fca126_mat1.0.115.gff3.gz
```

### Extract from the archive file
```bash
gzip -dk Felis_catus.F.catus_Fca126_mat1.0.115.gff3.gz
```

### Tell us a bit about the organism.

This genome annotation file belongs to a domesticated cat.

### How many sequence regions (chromosomes) does the file contain? Does that match with the expectation for this organism?
```bash
cat Felis_catus.F.catus_Fca126_mat1.0.115.gff3 | grep '##sequence-region' | wc -l
      70
```

The number 70 likely includes: The 19 autosomal chromosomes + the X chromosome. Additional unplaced or unlocalized scaffolds that are part of the reference but not integrated into the main chromosomal assembly.

### Check if the gff file has 9 columns
```bash
grep -v '^#' Felis_catus.F.catus_Fca126_mat1.0.115.gff3 | head -n 1 | tr '\t' '\n' | wc -l
       9
```

### How many features does the file contain?
```bash
grep -v '^#' Felis_catus.F.catus_Fca126_mat1.0.115.gff3 | cut -f3 | sort | uniq -c | sort -nr | wc -l
      22
```

### How many genes are listed for this organism?
```bash
cat Felis_catus.F.catus_Fca126_mat1.0.115.gff3 | cut -f3 | grep -w 'gene' | wc -l
      19209
```

### Is there a feature type that you may have not heard about before? What is the feature and how is it defined? (If there is no such feature, pick a common feature.)
```bash
grep -v '^#' Felis_catus.F.catus_Fca126_mat1.0.115.gff3 | cut -f3 | sort | uniq -c | sort -nr
      912206 exon
      717412 CDS                                                                                          
      336181 biological_region                                                                            
      116471 five_prime_UTR
      90775 three_prime_UTR                                                                               
      62351 mRNA                                                                                          
      29008 lnc_RNA
      19209 gene                                                                                          
      14844 ncRNA_gene                                                                                    
      1576 snRNA
      638 snoRNA                                                                                          
      397 pseudogenic_transcript                                                                          
      397 pseudogene
      221 miRNA                                                                                            
      70 region                                                                                           
      60 rRNA
      34 scRNA                                                                                            
      30 Y_RNA                                                                                            
      25 V_gene_segment
      20 transcript                                                                                       
      19 C_gene_segment
      5 J_gene_segment
```

* exon - The segments of a transcript that remain after splicing. This is the highest count because one gene usually has multiple exons.
* CDS(Coding Sequence) - The portion of the exons that actually translates into a protein (excludes UTRs).
* UTR (5' & 3') - Untranslated Regions -  Parts of the mRNA that are not translated into protein but are crucial for stability and regulation.
* mRNA(Messenger RNA) - The processed transcript that travels to the ribosome.
* lnc_RNA(Long non-coding RNA) - Transcripts longer than 200 nucleotides that do not code for proteins but regulate gene expression.
* pseudogene - Genomic sequences that resemble functional genes but have lost their protein-coding ability due to mutations.
* V/C/J segments - Components of the immune system (Immunoglobulins/T-cell receptors) that undergo somatic recombination.
---
- Exon vs. Gene: There are 912,206 exons but only 19,209 genes. This tells you that, on average, a gene in this dataset contains roughly 47 exons.
- CDS vs mRNA: There are more CDS entries than mRNA entries. This often happens because one mRNA can have multiple CDS regions annotated, or alternate start/stop sites are being tracked.
- The "ncRNA" world: The high number of lnc_RNA (29,008) compared to protein-coding genes (19,209) is a common feature in modern complex eukaryotic genome annotations.

### What are the top-ten most annotated feature types (column 3) across the genome?
```bash
grep -v '^#' Felis_catus.F.catus_Fca126_mat1.0.115.gff3 | cut -f3 | sort | uniq -c | sort -nr | head -n 10
912206 exon
717412 CDS                                                                                          
336181 biological_region                                                                            
116471 five_prime_UTR                                                                               
90775 three_prime_UTR
62351 mRNA                                                                                          
29008 lnc_RNA                                                                                       
19209 gene                                                                                          
14844 ncRNA_gene
1576 snRNA
```

### Having analyzed this GFF file, does it seem like a complete and well-annotated organism?

Yes, this is highly complete and professional-grade annotation, because the organism(we all love cats) is well-studied.
---
Several factors suggest this annotation is detailed:
* Presence of UTR-s: Many basic annotations only include exon and CDS, but getting UTR-s requires a lof of experimental work.
* Deep non-coding coverage: The presence of nearly 30,000 lnc_RNA entries and thousands of ncRNA_genes shows a focus on the "dark matter" of the genome. Draft annotations usually ignore these in favor of protein-coding regions.
* Pseudogenes
* Immune system elements: V_gene_segments and C_gene_segments, these are difficult to isolate and study.

### Share any other insights you might note.
### Isoform complexity
If we look at the relationship between genes and transcripts:
- Genes: 19,209
- mRNAs: 62,351
- Ratio: 3.25 mRNAs per gene.

This tells that the annotation accounts for significant alternative splicing. An incomplete annotation would have a ratio closer to 1:1.

### Exon density
With 912,206 exons for 62,351 mRNAs, we are looking at an average of 14.6 exons per transcript. This complexity is characteristic of vertebrates.