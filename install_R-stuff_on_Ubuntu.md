# Install-instructions on Ubuntu

## Access shared folder with host

- don't know, already forgot.
- enable a permanent shared folder on the host
- use Virtualbox-guestaddons
- is already present in Ubuntu-distributions (use `sudo apt install virtualbox-guest-additions-iso`)
- also check out if you need to download the guest addition iso on your host and then run the `autorun.sh`-script in the virtual CD drive


## Install conda/anaconda

- go the the [homepage](https://www.anaconda.com/download) and download the distribution (CAVE: big file, approx. 1.1GB)
- run the file (ends with `.sh` but is not just a script but contains a compressed binary), which installs the distribution
  - CAVE: you need to add the `execute` permission (use `chmod +x NAME_OF_FILE` for that)
- anaconda is installed on your local drive, the size will be approx. 7.5GB

You can run `conda update --all` to update everything.

Execute `conda activate TOOL_YOU_WANT_TO_USE` (prompt will change).
With `conda deactivate` you can go back to your base.


## Install R

First get the main setup for running basic R-stuff.

- use `sudo apt install r-base` for the basic installation
- start R using `R` in your terminal
- run `install.packages("tidyverse")` for the package installation.
  - this will fail because several system-wide dependencies are missing.
  - run `install.packages("tidyverse") a second time to view all the dependencies at once.
- Install them using within Ubuntu `sudo apt install libxml2-dev libfontconfig1-dev libssl-dev libcurl4-openssl-dev libharfbuzz-dev libfribidi-dev libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev xclip`


Then install the most important packages.

- from within R, run the following command: `if (!require("pacman")) install.packages("pacman"); library(pacman)`
- then run: `pacman::p_load(tidyverse, rio, here, skimr)`



## Install minimap2

- first clone the repository using `git clone https://github.com/lh3/minimap2`
- go into the folder (`cd minimap2`)
- run `make` to compile the project
- to execute the binary do one of the following:
  - use a complete path to the binary (e.g. `~/minimap2/minimap2`)
  - add this path to the $PATH-variable
    - add the following lines to your `~/.bashrc`:

```
  # manually add `minimap2` to the path variable
if [ -f "/home/saidou/minimap2/minimap2" ]; then
        export PATH="/home/saidou/minimap2:$PATH"
fi
```


## Install samtools

- either install it via the [official online source](https://github.com/samtools/samtools)
- for the lazy people: use `sudo apt install samtools`


## Install longshot

- you could try to use conda for that (2024-02-04: didn't work here)
  - additional infos from some hours later: maybe the problem was that we forgot to perform a `conda update --all`
- alternatively use the manual installation method described at the [project page](https://github.com/pjedge/longshot):
  - install dependencies using `sudo apt-get install cargo zlib1g-dev xz-utils libclang-dev clang cmake build-essential curl git`
  - git clone https://github.com/pjedge/longshot   # clone the Longshot repository
  - cd longshot                                    # change directory
  - cargo install --path .                         # install Longshot
  - export PATH=$PATH:/home/$USER/.cargo/bin       # add cargo binaries to path
  - echo 'export PATH=$PATH:/home/$USER/.cargo/bin' >> ~/.bashrc


## Install medaka

- follow the instructions [here](https://github.com/nanoporetech/medaka/blob/master/docs/installation.rst] for the installation
  - pip didn't work (2024-02-04)
  - with conda (using `conda create -n medaka -c conda-forge -c bioconda medaka`) 

## to download the referance genome, 
we are going to e.g
1. cd Experiment1/barcode02/analysis02/less SARS_CoV.merged.vcf.gz
2. copy MN908947.3 qnd paste on google and download sequence.fasta rename it to MN908947.3.fasta
3. create new directory colled reference genome
4. execute de commqnd line following tutorial https://artic.network/ncov-2019/ncov2019-bioinformatics-sop.html
5. 


## look NANOPORE not easy if our Instute not partenership

## to install Guppy command: tar -xf ont-guppy-cpu_6.5.7_linux64.tar.gz


## individual fasta.fast is wich command 


# manually add `guppy` to the path variable
```
if [ -f "/home/saidou/ont-guppy-cpu/bin/ont-guppy-cpu/bin" ]; then
	export PATH="/home/saidou/ont-guppy-cpu/bin:$PATH"
fi


export PATH="/home/saidou/.cargo/bin:/home/saidou/ont-guppy-cpu/bin:$PATH"
```

## 

```
source activate artic-ncov2019

artic guppyplex --min-length 400 --max-length 700 --directory /home/saidou/Documents/MUW_analysis/NGS-Results/Experiment1/barcode02 --prefix barcode02

artic guppyplex --min-length 400 --max-length 700 --directory /home/saidou/Documents/MUW_analysis/NGS-Results/Experiment1/barcode02 --prefix barcode02
Processing 15 files in /home/saidou/Documents/MUW_analysis/NGS-Results/Experiment1/barcode02
barcode02_barcode02.fastq	21222
```


artic minion --normalise 200 --threads 4 --scheme-directory ~/home/saidou/Documents/MUW_analysis/NGS-Results/Experiment1/barcode03 --read-file FAP33325_pass_barcode05_ac8ca151_0.fastq --fast5-directory path_to_fast5 --sequencing-summary path_to_sequencing_summary.txt nCoV-2019/V3 samplename


## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## 2023-02-06 ## ## ## ## ## ## ## ## ## 
## which command to generate the bam files 

A BAM file is just a compressed SAM file. The file begins with a header, which is optional.The header is used to describe source of data, reference sequence, method of alignment, etc., this will change depending on the aligner being used.
```
~/Documents/MUW_analysis/NGS-Results/Experiment1/barcode02/analysis02/$ samtools view -H SARS_CoV.primertrimmed.rg.sorted.bam
```

# indexing
minimap2 -d ref.mmi SARS_CoV_analysis02.fastq

# alignment
minimap2 -a ref.mmi SARS_CoV_analysis02.fastq > alignment.sam 

# for PacBio CLR reads
minimap2 -ax map-pb  MN908947.3.fasta SARS_CoV_barcode02.fastq > aln.sam

# for Oxford Nanopore reads
minimap2 -ax map-ont MN908947.3.fasta SARS_CoV_barcode02.fastq > aln.sam

# for Illumina Complete Long Reads
minimap2 -ax map-iclr MN908947.3.fasta SARS_CoV_barcode02.fastq > aln.sam


samtools view -h SARS_CoV.primertrimmed.rg.sorted.bam | less

minimap2 -a -x map-ont -t 4 /home/saidou/Documents/MUW_analysis/NGS-Results/Experiment1/barcode02/analysis02/MN908947.3.fasta SARS_CoV_guppyplex_fastq_pass-barcode02.fastq


## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## 2023-02-07 ## ## ## ## ## ## ## ## ## 

## longshot for variant calling

## create new fille ~/Documents/MUW_analysis/analysis_Vienna/barcode02
```
source activate artic-ncov2019
artic guppyplex --min-length 400 --max-length 700 --directory /home/saidou/Documents/MUW_analysis/NGS-Results/Experiment1/barcode02 --prefix barcode02
minimap2 -a -x map-ont -t 4 /home/saidou/Documents/MUW_analysis/NGS-Results/Experiment1/barcode02/analysis02/MN908947.3.fasta barcode02_barcode02.fastq > barcode02.sam 	# create sam files
less barcode02.sam
samtools view
samtools view -b barcode02.sam > barcode02.bam 																	# convert sam to bam files
less barcode02.bam
samtools sort barcode02.bam > barcode02_sorted.bam 																# sorted the bam files and name it barcode02_sorted
samtools index barcode02_sorted.bam  																		# to index my barcode02_sorted.bam, qfter execution we optain barcode02_sorted.bam.bai
																						# I am going for longshot: https://github.com/pjedge/longshot
longshot --bam barcode02_sorted.bam --ref /home/saidou/Documents/MUW_analysis/NGS-Results/Experiment1/barcode02/analysis02/MN908947.3.fasta --out longshot_barcode02.vcf
cat longshot_barcode02.vcf 																			# to view longshot_barcode02.vcf files
cat longshot_barcode02.vcf | grep -v "^#"  																	# to view and exclude the line contain # sympol in  the ongshot_barcode02.vcf files
cat longshot_barcode02.vcf | grep -v "^#" | wc -l 																# to view and exclude the line contain # sympol in  the ongshot_barcode02.vcf files and count
```


## medaka for fasta consensus generation / sequence polishing:



## 






















