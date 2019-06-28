# Introduction

This repository follows the protocol outlined in [Nature Journal](https://www.nature.com/articles/nprot.2016.095) with additional preprocessing and analysis steps. The ultimate goal of this pipeline is to process raw RNAseq data and compute differential expression analysis between experimental conditions.

Within this repository, you will find software and methods for completing the above task.

## Overview of Steps
### Quality Control:

1. Demultiplex sequencing data to group by sample 
2. Perform quality control check on raw RNA read data

### Main RNAseq Pipeline Steps:

1. Alignment of reads to a known reference genome
2. Assembly of alignments into full-length transcripts
3. Quantification of gene and transcript expression levels. Export in a Ballgown readable format
4. Calculation of differential gene and transcript expression across different experimental conditions
5. Visualization of Data

## Software Necessary

* [Demultiplexer](https://github.com/NuttyLogic/Demultiplexer)
	- __requires python > 3.4__
	- Download from GitHub repository, extract, and run
* [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
	- __requires Java__
	- Quality Control 
* [MultiQC](https://multiqc.info)
	- __requires python__
	- Quality Control sample summary report
* [HISAT2](https://ccb.jhu.edu/software/hisat2/index.shtml)
	- Accomplishes step 1
* [SAMtools](http://www.htslib.org/)
* [StringTie](https://ccb.jhu.edu/software/stringtie/index.shtml?t=manual)
	- Accomplishes steps 2 and 3
* [gffcompare](http://ccb.jhu.edu/software/stringtie/gff.shtml#gffcompare)
* [R](https://www.r-project.org)
	- Data visualization. Required to use Ballgown
	* Packages Necessary:
		- devtools
		- [Ballgown](https://github.com/alyssafrazee/ballgown)
			- Accomplishes step 4
		- [RSkittleBrewer](https://github.com/alyssafrazee/RSkittleBrewer)
		- genefilter
		- dplyr
		- ggpubr
		- corrplot
***

# Software Installation and Setup

__NOTE:__ All instructions are under the assumption that work is being done on a Linux (Ubuntu) machine and commands will either be run from a __Unix shell prompt__ from a terminal window or from the R environment. 

To be able to access exec commands from any location in the terminal without having to specify paths, create two directories in which one stores downloaded and installed software directories and the other contains copies of the executables added to your `$PATH` variable

### Make Storage Directory
```
mkdir -p ~/rnaseq/
```
### Make Executables Directory
```
mkdir -p ~/bin/
```
### Add Executables Directory to $PATH
Add the following commands to your ~/.profile file to make these changes permanent.
__Note: Some systems have already added this directory to PATH once the directory is made__

Navigate to `~/.profile` and edit with `vim` to edit `$PATH` variable
```
cd ~/
vim .profile
```
Once in `vim`, hit "i" on your keyboard to enter "__INSERT__" mode and add the following to your `~/.profile` file

```
PATH="$HOME/bin:$PATH"
```
Hit the "esc" key to quit "__INSERT__" mode. Type `:wq` to write to `~/.profile` and quit `vim`

## Download and Install Software
The following instructions will install the software listed above and copy the necessary executables to the Executables folder

__Note:__ commands below may not install the latest software. The links provided above can be used to verify and download the latest versions

### Demultiplexer
Download from GitHub linked above

### FastQC

**_Note: FastQC requires a Java Runtime Environment (JRE) to run. On OSX, a Java Development Kit (JDK) is also required._**

```
cd ~/rnaseq/

wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.8.zip --no-check-certificate
unzip fastqc_v0.11.8.zip

cd FastQC/
chmod 755 fastqc
```
To run FastQC from anywhere, make a link to the original wrapper script:
```
sudo ln -s /path/to/FastQC/fastqc $HOME/bin/fastqc
```
The path to the wrapper script should be `/rnaseq/FastQC/fastqc` , if placed in the directory as shown above.

### MultiQC

It is easiest to install MultiQC with pip:

```
pip install multiqc
```

To update:
```
pip --update multiqc
```
### SAMtools
Download, unpack, and configure 
```
cd ~/rnaseq/

wget https://github.com/samtools/samtools/releases/download/1.9/samtools-1.9.tar.bz2
bunzip2 samtools-1.9.tar.bz2
tar -xvf samtools-1.9.tar

cd samtools-1.9
make
./samtools

cp samtools-1.9/samtools $HOME/bin
```
### HISAT2
```
cd ~/rnaseq/

wget ftp://ftp.ccb.jhu.edu/pub/infphilo/hisat2/downloads/hisat2-2.1.0-Linux_x86_64.zip
unzip hisat2-2.1.0-Linux_x86_64.zip
cd hisat2-2.1.0

cp hisat2-2.1-0/hisat2* hisat2-2.1.0/*.py $HOME/bin
```

### StringTie
```
cd ~/rnaseq/

wget http://ccb.jhu.edu/software/stringtie/dl/stringtie-1.3.4d.Linux_x86_64.tar.gz
tar -xzvf stringtie-1.3.4d.Linux_x86_64.tar.gz

cp stringtie-1.3.4d.Linux_x86_64/stringtie $HOME/bin
```
### gffcompare
```
cd ~/rnaseq/

wget http://ccb.jhu.edu/software/stringtie/dl/gffcompare-0.10.6.Linux_x86_64.tar.gz
tar -xzvf gffcompare-0.10.6.Linux_x86_64.tar.gz

cp gffcompare-0.10.6.Linux_x86_64.tar.gz/gffcompare $HOME/bin
cp gffcompare-0.10.6.Linux_x86_64.tar.gz/trmap $HOME/bin
```
### R
Install the R package by adding the CRAN repository
```
sudo apt install apt-transport-https software-properties-common

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/'

sudo apt update
sudo apt install r-base

sudo apt install build-essential
```
To verify that installation was successful, run `R --version`

#### devtools
Install dependencies for devtools and start an R session as a root user
```
sudo apt install build-essential libcurl4-gnutls-dev libxml2-dev libssl-dev

sudo -i R
```
Install devtools in R Session
```
install.packages('devtools')
```
#### Ballgown
```
source("http://bioconductor.org/biocLite.R")
biocLite("ballgown")
```
#### RSkittleBrewer
```
devtools::install_github('alyssafrazee/RSkittleBrewer')
```
#### genefilter, dplyr
```
biocLite(c("genefilter", "dplyr"))
```
#### ggpubr
From GitHub:
```
devtools::install_github("kassambara/ggpubr")
```
Or, from CRAN:
```
install.packages("ggpubr")
```

#### corrplot
```
install.packages("corrplot")
```

# STEP 0: Quality Control

**This step should be completed prior to alignment steps**

The primary goal of quality control is to ensure the samples moving forward for downstream analysis are of substantial quality. 

## Software Used:
1. FastQC
2. MultiQC

### Any additional information and option descriptions can be found on the [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/) and [MultiQC](https://multiqc.info) websites.

## FastQC Setup

**_Note: FastQC requires a Java Runtime Environment (JRE) to run. On OSX, a Java Development Kit (JDK) is also required._**

This pipeline runs FastQC in the **non-interactive** mode. Running just `fastqc` will try to open the graphical user interface.

To run FastQC on multiple files, simply list the names of the files and indicate the output directory

```
fastqc -o /path/to/output/directory file1.fastq file2.fastq file3.fastq ...
```
**Options that are particularly useful**

* `-o/--outdir </path/to/output/directory>` : Specifies the directory to write HTML output files to
* `-f/--format` : Specifies the file type. Valid formats are bam, sam, bam_mapped, sam_mapped and fastq. There is normally file format detection, but you can specify to be safe.

Running `fastqc --help` will give a full list of available options

There is also a video tutorial available [here](https://www.youtube.com/watch?v=bz93ReOv87Y).

**Detailed descriptions of analysis modules results can be found [here](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/).**

## MultiQC

To run MultiQC, simply navigate to the desired analysis directory and run:

```
multiqc .
```
You can also specify a specific directory or directories:
```
multiqc /path/to/directory1/ /path/to/directory2/
```

**Option that is particularly useful**

* `-x/--ignore <string>` : Specifies the string used to match to files, paths, and directories to ignore. This can be specified multiple times.

***

# STEP 1: Alignment to a Known Reference Genome

**Initial Quality Control measures should have been taken to filter out low-quality sample sets.**

This RNAseq pipeline starts with alignment of the obtained RNA reads to a reference genome with **HISAT2**.

We will also convert the outputed SAM files to BAM files using **SAMtools**.

## Software Used:
1. HISAT2
2. SAMtools

### Any additional information and option descriptions can be found in the [HISAT2](http://ccb.jhu.edu/software/hisat2/manual.shtml) and [SAMtools](http://www.htslib.org/doc/samtools.html) manuals.

## Building an Index for Alignment

Some organisms have prebuilt indexes available on the HISAT2 website, while others require you to build an index yourself. If a prebuilt index is available, it is generally encouraged to utilize the index rather than building your own, as you will result in an identical index. 

Additionally, building an index can require significantly more memory than the actual alignment step. If you wish to use the `--ss` or `--exon` option to build an index that considers known splice-site and exon information from the genome annotation file (`.gtf` format), it can require about 160 GB of RAM for the whole human genome.

Excluding these options, you can build an index for alignment on a regular 8GB desktop computer with the following command. 

```
hisat2-build [options] reference/genome/file.fa ht2_base
```
Here, `ht2_base` is the base name for the .ht2 output files. For example, if ht2_base = chr19, the index files would be named chr19.1.ht2, chr19.2.ht2, ...

FASTA reference genome files can be obtained from many sources. A popular option is to download the reference genome for your organism of choice from the [Ensembl](https://www.ensembl.org/info/about/mirrors.html) website (Choose your preferred mirror site from that link). [UCSC](http://genome.ucsc.edu/cgi-bin/hgGateway) and [NCBI](http://www.ncbi.nlm.nih.gov/sites/genome) are additional options.

You can use the following command to extract information about the built index

```
hisat2-inspect [options] ht2_base
```
The `ht2_base` will be the name of the files HISAT2 will search for (first in the current directory, then the directory specified in the `HISAT2_INDEXES` environmental variable. 

## Alignment with Hisat2

The general usage of hisat2 is as follows:

```
hisat2 [options] -x </index/base/name> -S </file/to/write/SAM/alignments/to> {-1 <r1> with -2 <r2>, -U <r>, or --sra-acc <SRA accession number>}
```
The inputs in the { } are used to specify the types of read files used. For example, `-1` is used with `-2` for paired-read analysis while `-U` is used for inputing unpaired read files.

The `-x` option searches for the basename for the index files to be used. Can be specified as a path. Do **not** include the `.x.ht2` extention in the base name.

The default file format for reads is `FASTQ`; options can be used to specify the format used if necessary.

**_Note: Each sample / experimental condition should be aligned with its own, separate hisat2 command_**

**Options that are particularly useful**

* `-p/--threads NTHREADS` : Specifies the number of parallel search threads to launch

* `--dta/--downstream-transcriptome-assembly` : The output alignments are tailored for transcript assemblers (Like StringTie!) Using this option can help improve computation and memory usage

* `--known-splicesite-infile <path>` : Provides a list of known splice sites to HISAT2. This can be used if the `--ss` option was not used in building the index. **_Although it can be useful in the alignment process, it is preferable to use indexes built with annotated information._** This is helpful with if that option is not feasible

* `--summary-file <file>` : Write the alignment summary to this file. Alignment summary will still print out in the terminal, but will also be written to this file for future reference

## Sorting and Converting SAM files to BAM files

After the RNA reads have been aligned to the refrence genome, it is necessary to sort by chromosomal coordinates and convert the SAM alignment files to BAM format. BAM files are the binary equivalent of the human-readable SAM files

The following command will sort and convert the files:

```
samtools sort [options] -o /output/file1.bam /input/file1.sam
```
**Options that are particularly useful**

* `-o <out.bam>` : In the above command, `-o` is technically an option, not a required input as `samtools sort` can be used just to sort the input file and write the sorted output to standard output. This option's default output format is `.bam`, but can be specified with `-O <FORMAT>` if wanted
* `-@ <INT>` : Set the number of threads used for sorting and converting

### `samtools view`

This command can be used to print all allignments from the input file to standard output. 

Using the `-c` option will instead count all the alignments and print the number to standard output. 

This can be used to double check that the conversion from SAM to BAM was executed correctly, as the number of alignments should be conserved between the two file types.

```
samtools view [options] -c /input/file1.sam

samtools view [options] -c /input/file1.bam
```
These two commands should return the same number.

`samtools view` can also be used to convert SAM files into an unsorted BAM output with the following command:

```
samtools view -bS file1.sam > file1.bam
```
**Options that are particularly useful**

* `-@ <INT>` : Set the number of threads used for BAM compression
* `-S` : This option may be uneccesary depending on the version of SAMtools being used; refer to the SAMtools manual for more information

***

# STEP 2 & 3: Assembly of Alignments into Full-Length Transcripts and Expression Quantification

**SAM alignment output from HISAT2 should have already been sorted and converted to BAM with SAMtools.**

The next step of the pipeline involves the assembly of the read alignments outputted by HISAT2 into full-length transcripts using **StringTie**.

## Software Used:
1. StringTie

### Any additional information and option descriptions can be found in the [StringTie](https://ccb.jhu.edu/software/stringtie/index.shtml?t=manual) manual.


## Workflow Options:

Because StringTie has the ability to discover and assembly novel transcripts not present in the refrence annotation, there are two workflow options that are primarily followed when using StringTie. The difference lies in the goal of the RNAseq project and whether novel isoforms are of interest:

### Option 1:

1. Run StringTie for each sample BAM file. Provide the genome annotation file if available with the `-G` option
2. Run StringTie with the `--merge` function to merge all sample transcripts generated into a set of _non-redundant_ transcripts
3. Run StringTie with the `-e` and `-B/-b` option to estimate transcript abundances and output data into a _Ballgown readable format_ for further downstream analysis. Instead of specifying the Reference Genome Annotation file to option `-G`, use the merged transcripts `.gtf` file generated from the `stringtie --merge` step
4. The output can now be read in by Ballgown for generating plots, differential expression analysis, etc.

### Option 2:

This workflow is a simplified, quicker version in which there is no interest in novel isoforms. This workflow **does not** individually assembly each sample's transcript and then merge them. This workflow estimates and analyzes expression based on only the transcripts that are found in the reference genome annotation.

1. Run StringTie with the `-e`, `-B/-b` and `-G` option for each sample BAM file. The `-G` option will take the Reference Genome Annotation `.gtf` file, **NOT** the merged transcript file. ( You would not have generated this anyway with this workflow ).
2. The output can now be read in by Ballgown for generating plots, differential expression analysis, etc.

## General Usage:

```
stringtie [options] aligned_reads.bam
```
**Options that are particularly useful**

* `-G <ref_anno_file>` : Specifies the reference annotation file (`GTF` or `GFF3` format) used to guide the transcript assembly. Required by options `-e, -B/-b, -C`
* `-o </path/out.gtf>` : Specifies the name of the output `GTF` file. Can be inputted as a path, directories will be created if needed
* `-e` : Limits transcripts estimated and outputted to only the ones matching the annotation file passed to option `-G`
* `-B` and `-b <path>` : Switches output to be _Ballgown_ input tables. `-b <path>` will output the data into the specified path/directory instead of the directory specified in the `-o` option
* `-p <int>` : Specifies the number of processing threads to use for transcript assembly

***




# Amazon Web Services (AWS) Setup


