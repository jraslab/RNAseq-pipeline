# Introduction
Software and methods for a bioinformatics pipeline

This repository follows the protocol outlined in [Nature Journal](https://www.nature.com/articles/nprot.2016.095) with additional preprocessing and analysis steps

### Quality Control

* Demultiplex Sequencing data to group by sample
* Perform quality control check on raw RNA read data

### Main RNAseq Pipeline Steps

1. Alignment of reads to a known reference genome
2. Assembly of alignments into full-length transcripts
3. Quantification of gene and transcript expression levels. Export in a Ballgown
4. Calculation of differential gene and transcript expression across differnt experimental conditions

### Software Necessary

* [NuttyLogic/Demultiplexer](https://github.com/NuttyLogic/Demultiplexer)
* [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
	- Quality Control 
* [HISAT2](https://ccb.jhu.edu/software/hisat2/index.shtml)
	- Accomplishes step 1
* [SAMtools](http://www.htslib.org/)
* [StringTie](https://ccb.jhu.edu/software/stringtie/index.shtml?t=manual)
	- Accomplishes steps 2 and 3
* [gffcompare](http://ccb.jhu.edu/software/stringtie/gff.shtml#gffcompare)
* R
	- Data visualization. Required to use Ballgown
	* Packages Necessary:
		- devtools
		- [Ballgown](https://github.com/alyssafrazee/ballgown)
			- Accomplishes step 4
		- [RSkittleBrewer](https://github.com/alyssafrazee/RSkittleBrewer)
		- genefilter
		- dplyr
		- ggpubr
		


# Software Installation and Setup

__NOTE:__ All instructions are under the assumption that work is being done on a Linux (Ubuntu) machine and commands will either be run from a __Unx shell prompt__ from a terminal window or from the R environment. Commands that should be run from the Unix shell are indicated with a `'$'` character.  R scripts are indicated with a `'>'` character.

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
__Note: Some systems have already added this to directory to PATH once the directory is made__

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
The following instructions will Install the software listed above

### Demultiplexer
### FastQC
### SAMtools
### HISAT2
### StringTie
### gffcompare
### R
Install the R package by adding the CRAN repository 
```
$ sudo apt install apt-transport-https software-properties-common

$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
$ sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/'

$ sudo apt update
$ sudo apt install r-base

$ sudo apt install build-essential
```
To verify that installation was successful, run `$ R --version`

#### devtools
Install dependencies for devtools and start an R session as a root user
```
$ sudo apt install build-essential libcurl4-gnutls-dev libxml2-dev libssl-dev

$ sudo -i R
```
Install devtools
```
> install.packages('devtools')
```
#### Ballgown
```

```
#### RSkittleBrewer
```

```
#### genefilter
```

```
#### dplyr
```

```
#### ggpubr
```
> install.packages("ggpubr")
```


# Amazon Web Services (AWS) Setup


