# WGS
## Quality control of raw sequencing data for clean data filteration
### Raw Data
The original sequencing data acquired by high-throughput sequencing platform(e.g. Illumina HiseqTM/NovaSeqTM) recorded in image files are firstly transformed to sequence reads by base calling with CASAVA software. The sequences and corresponding sequencing quality information are stored in a FASTQ file.
### Sequencing Data quality control
#### Sequencing Quality Distribution
#### Distribution of sequencing errors 
#### Sequencing Data Filtration
Raw data obtained from sequencing contains adapter contamination and low-quality reads. These sequencing artifacts may increase the complexity of further analyses, so we utilize quality control steps to remove them. Consequently, all the further analyses are based on the clean reads. The quality control steps are as follows.
- Discard the paired reads when either read contains adapter contamination
- Discard the paired reads when uncertain nucleotides (N) constitute more than 10 percent of either read
- Discard the paired reads when low quality nuccleotides (base quality less than 5, Q<= 5) constitute more than 50 percent of either read.
### Statistics of sequencing data
### Q&A
Q: As the sequencing error increases with the read length, what is the acceptable range of sequencing error rate?
A: Currently for Illumina sequencers, the per base error rate is generally lower than 1% and the highest acceptable threshold is 6%.


## Mapping clean reads to reference genome
The effective sequencing data were aligned with the reference sequences through BWA software(parameters: mem -t 4 -k 32 -M), and the mapping rate and coverage were counted according to the alignment results. The duplicates were removed by SAMTOOLS.
### Statistics of Reference Genome
### Mapping Statistics with Reference Genome
### Mapping Summary
### Q&A
Q: What are the potential causes of low mapping rate?
A: The three major causes are: (1) with poorly assembled reference genome or a relatively far genetic relationship between the reference and sample; (2) treatments to DNA that may change the sequence (e.g. bisulfite treatment) and herein affecting the mapping rate; (3) contanimation of other species.
## SNP, Indel, SV and CNV detection and annotation according to the mapping results
### SNP detection & Annotation
Single nucleotide polymorphism (SNP) refers to the variation in a single nucleotide which may occure at some position in the genome, including transition and transversion of a single nucleotide. We detected the individual SNP variations using SAMtools with the following parameter:'mpileup -m 2 -F 0.0002 -d 1000'
To reduce the error rate in SNP detection, we filtered the results with the criterion as follows:
- the number of support reads for each SNP should be more than 4;
- The mapping quality (MQ) OF each SNP should be higher than 20.
#### Statistics of SNP Detection & Annotation
ANNOVAR is a widely used software in variation annotation with multiple capabilities, including gene-based annotation, region-based annotation as well as other functionalities.
Upstream: SNPs located within 1 kb upstream (away from transcript start site) of the gene
Exonic: SNPs located in exonic region; Non-synonymous: single nucleotide mutation with changing amino acid sequence; stop gain/loss: a nonsynonymous SNP that leads to the introduction/removal of stop codon at the variant site; Synonymous: single nucleotide mutation without changing amino acid sequence.
Intronic: SNP located in intronic region;

### InDel Detection & Annotation
### SV Detection & Annotation
### CNV Detection & Annotation
### Visualization of Variation
