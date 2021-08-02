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
Splicing: SNPs located in the splicing site(2bp range of the intron/exon bundary)
Downstream: SNPs located within 1 kb downstream (away from transcription termination site) of the gene region.
Upstream/Downstream: SNPs located within the <2 kb intergenic region, which is in 1 kb downstream or upstream of the genes.
Intergenic: SNPs located within the > 2 kb intergenic region.
ts: Transitions, a point mutation that changes a purine nucleotide to another purine or a pyrimidine nucleotide to another pyrimidine. Approximately two out of three SNPs are transitions.
tv:Transversions, the substitution of a (two ring) purine for a (one ring) pyrimidine or vice versa.
ts/tv: The ratio of transitions to transversions
Het rate: Genome-wide heterozygous rate, calculated by the ratio of heterozygous SNPs to the total number of genome bases.
Total: The total number of SNPs.
#### SNP Quality Distribution
To access the credibility of detected SNPs, we checked the distribution of support reads number, SNP quality, as well as the distance between adjacent SNPs.
#### SNP mutation Frequency
Taken the T:A>C:G mutations as an example, this category includes mutations from T to C and A to G. When T>C mutation appears on either of the double-strand, the A>G mutation will be found in the same position of the other chain. Therefore the T>C and A>G mutations are classified into one category. Accordingly, the whole-genome SNP mutations could be classified into six categories.
#### Q&A
Q: What  are transitions and tranversions?
A: transitions refer to the changes between A and G, which are both purines, or between C and T, which are both pyrimidines; while transversions repretent changes between a purine and a pyrimidine, such as between A and T.
Q: If the PCR-sequencing method failed to verify the detected snp, did this mean that NGS SNP calling is not reliable?
A: SNP calling in NGS is based on the support reads and depends on the sufficient coverage depth, which ensures the accuracy of most but not all the detected SNPs. We recommend to double-check the PCR results first, and then use a genome browser such as Savant and IGV to manually check the mapped reads of the NGS result.


### InDel Detection & Annotation
InDel refers to the insertion or deletion of <= 50 bp sequences in the DNA. SAMtools was used to detect InDel using parameter"mpileup -m 2 -F 0.002 -d 1000" to dect InDels and followed by annotation using ANNOVAR. 
Upstream: InDels located within 1 kb upstream (away from transcript start site) of the gene
Exonic: InDels located in exonic region; Stop gain/loss: InDels leads to the introduction/removal of stop codon at the variant site; Synonymous: Frameshift deletion/insertion: InDel mutation changing the open reading frame with deletion or insertion; Non-Frameshift deletion/insertion: InDel mutation without changing the open reading frame with deletion or insertion sequence of 3 or multiple of 3 bases.
Intronic: InDel located in intronic region;
Splicing: InDel located in the splicing site(2bp range of the intron/exon bundary)
Downstream: InDel located within 1 kb downstream (away from transcription termination site) of the gene region.
Upstream/Downstream: InDel located within the <2 kb intergenic region, which is in 1 kb downstream or upstream of the genes.
Intergenic: InDel located within the > 2 kb intergenic region.
Het rate: Genome-wide heterozygous rate, calculated by the ratio of InDels to the total number of genome bases.
Total: The total number of InDels.

####
Length Distribution of CDS-located InDels
####
Q: What is the heterozygous rate for an InDel?
A: Ratio of heterozygous InDels to total number of InDels, the heterozygous Indel only located in one of the homologous chromosomes of diploid sample.

### SV Detection & Annotation
Structual variants (SVs) are genomic variation with mutations of relatively larger size (> 50 bp), including deletions, duplications, insertions and translocations. BreakDancer software was used to detect insertion (INS), deletion(DEL), inversion(INV), intra-chromosomal translocation(ITX) and inter-chromosomal translocation(CTX) mutations, based on the reference genome mapping results and the detected insert size. The detected SV were filtered by removing those with less than 2 supporting PE reads. The INS, DEL and INV were futher annotated by ANNOVAR.
#### Statistics of SV Detection & Annotation
Upstream: SVs located within 1 kb upstream (away from transcript start site) of the gene
Exonic: SVs located in exonic region; 
Intronic: SVs located in intronic region;
Splicing: SVs located in the splicing site(2bp range of the intron/exon bundary)
Downstream: SVs located within 1 kb downstream (away from transcription termination site) of the gene region.
Upstream/Downstream: SVs located within the <2 kb intergenic region, which is in 1 kb downstream or upstream of the genes.
Intergenic: SVs located within the > 2 kb intergenic region.
INS: Insertion
DEL: Deletion
INV: Inversion
ITX: Intra-chromosomal translocations
CTX: Inter-chromosomal translocations
Total: The total number of SVs.
#### Length Distributio of SVs
#### SV Detection Q&A
Q: How did the SV's detected?
A: There are four strategies for SV detection: (1) the read-pair technology, with detection of insertional or deletional mutations via aberrant insert size, and inversion via incorrect reading direction; (2) split-read approaches, which detects SVs with uncontinously mapped reads on different positions of the reference genome; (3) the read-depth method, detecting CNV caused by insertions and deletions; (4) de novo sequence assembly, with detection of SVs by comparison between the assembly and the reference genome. Currently, the SV detection softwares are generally based on one of the principles, the Breakdancer software works with the read-pair method to detect the SVs.


### CNV Detection & Annotation
Copy-number variation (CNV) is a type of structural vairation showing deletions or duplications in the genome. Based on the reads depth of the reference, CNVnator were used to detect CNVs of potential deletions and duplications with the following paramenter "-call 100". The detected CNV were further annotated by ANNOVAR.
Upstream: CNVs located within 1 kb upstream (away from transcript start site) of the gene
Exonic: CNVs located in exonic region; 
Intronic: CNVs located in intronic region;
Downstream: CNVs located within 1 kb downstream (away from transcription termination site) of the gene region.
Upstream/Downstream: CNVs located within the <2 kb intergenic region, which is in 1 kb downstream or upstream of the genes.
Intergenic: CNVs located within the > 2 kb intergenic region.
Duplication: CNVs with increased copy number
Deletion: CNVs with decreased copy number
Duplication length (bp): Total length of CNV duplication
Deletion length (bp): Total length of CNV deletion
Total: The total number of CNVs.
#### Q&A
Q: What's the strategy for CNV detection?
A: In brief, CNVs are called using read depths of bins, with involving GC calibration and sequencing uniformity calibration.
### Visualization of Variation
For proper visualization of the structural variations on the whole-genome, we present according to mutation types with Circos
(1) for SNP/InDel type, the density distribution is drawn
(2) for SV/CNV type, the lcoation and size are drawn.
