##Mapping Sequence Data 

###Introduction
A wide array of Next Generation Sequencing platforms are available nowadays and, depending on the platform, a single machine can output a total of 6000 Gbp in approximately 40h (the equivalent to approximately 937 human diploid genomes). Moreover, this data is produced in the form of short or long sequencing reads, also depending on the platform [1]. Given the ability of these machines to produce millions of reads from a single organism, the process of arranging these in an attempt to find overlaps and delineate contigs (genome assembly) is computationally demanding. Long read lengths are more adequate for genome assembly whereas the assembly of a genome using short read lengths may prove challenging particularly due to repetitive regions. This challenge can be partially overcomed using a reference genome for the organism being studied, in which the sequencing reads are aligned with this reference genome. This procedure is often called reference assembly or mapping. This designation contrasts with the procedure in which sequencing reads are assembly without a reference genome – de novo assembly.  An important aspect for mapping that should be taken in account is that the reference genome should be a high quality well assembled genome [1].
This module will address the basic bioinformatic analytical steps underlying mapping/reference assembly of NGS data. We will mainly focus on the analysis of sequence data obtained from Illumina platforms, that presently dominates 80-90% of the market, sequenced in paired-end mode. What does “paired-end mode” mean?  It is important to first start by understanding the process of Illumina “sequencing-by-synthesis” methodology: starting with genomic DNA, it is randomly sheared and an appropriate length is selected after fractioning by agarose gel electrophoresis, followed by adapter ligation and solid phase PCR amplification (bridge amplification) on a sequencing flow cell [1]. Afertwards, sequencing by synthesis is carried out using reversible fluorescently-labelled terminator nucleotides where each fragment is sequenced on both ends producing two mate reads for each fragment (hence, paired-end sequencing). The size of the region that is not sequenced corresponds to the region between reads in this fragment and is often called insert size and therefore depends on the read length (which depends on the machine and number of cycles, currently up to 2x300bp on MiSeq).
The ensuing computational or in silico analysis picks up from the FASTQ files produced after adapter removal. The two main approaches at this point are as mentioned, Mapping and De novo Assembly. In this module we will focus on Mapping sequence reads obtained from M. tuberculosis clinical isolates collected in Portugal. By mapping sequence reads to a reference genome it will enable the downstream identification of Single Nucleotide Polymorphisms (SNPs), insertions and deletions (indels) and copy number variants (CNVs) that exist between the two organisms. Comparative Genomics underpinned on mapping analysis is also possible if the reference sequence remains the same. 

The general workflow for this analysis consists in mapping the reads against a reference sequence using a mapper/aligner software to produce a mapping file (SAM/BAM) that can be further analysed and sorted using programs such as Picard Tools/Genome Analysis Toolkit or Samtools. Afterwards, it is possible to visualize the alignments using appropriate software with graphical interface and perform additional downstream analysis such as variant calling.
First, let’s have a quick look at the FASTQ file format (Exercise 1).




Exercise 1 – Understanding the FASTQ file format
Let’s have a look at the files made available for this course. Under the home directory there is a another directory called fastq_files.
Let´s access this folder: open up a terminal and type:

`$ cd course_files`

You can list its contents by typing ls (list command) which should give you the list of files and sub-directories within. You’ll find a fasta file (NC000962_3.fasta) for the M. tuberculosis H37Rv reference genome (GenBank Acc. NC000962.3) and ten compressed fastq files for five different M. tuberculosis clinical isolates:
•	PT000033:  PT000033_1.fastq.gz and PT000033_2.fastq.gz
•	PT000049:  PT000049_1.fastq.gz and PT000049_2.fastq.gz
•	PT000050:  PT000050_1.fastq.gz and PT000050_2.fastq.gz
•	PT000271:  PT000271_1.fastq.gz and PT000271_2.fastq.gz
