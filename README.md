# Introduction
Software and methods for a bioinformatics pipeline

This repository follows the protocol outlined in [Nature Journal](https://www.nature.com/articles/nprot.2016.095) with additional preprocessing steps

### Quality Control

* Perform quality control check on raw RNA read data

### Main RNAseq Pipeline Steps

1. Alignment of reads to a known reference genome
2. Assembly of alignments into full-length transcripts
3. Quantification of gene and transcript expression levels. Export in a [Ballgown](https://github.com/alyssafrazee/ballgown)
4. Calculation of differential gene and transcript expression across differnt experimental conditions

#### Software Necessary

* FastQC
	- Preprocessing Step
* HISAT2
	- Completes step 1
* StringTie
	- Completes steps 2 and 3
* R
	- Data visualization. Required to use Ballgown
* Ballgown
	- Accomplishes step 4


# Amazon Web Services (AWS) Setup


