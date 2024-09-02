# Accelerating Microbial Insight with HPC and Bioinformatics

This instruction provides an example of analysing microbial data using MAHAMERU BRIN HPC. In this workshop, all the software that will be used is already available as a module, hence you don't need to prepare the environment. The snippet code on this page is designed to be copied by clicking on the right corner of each code box and pasting it in your terminal.

## Login to MAHAMERU
Open your terminal (Linux, MacOS) or PowerShell (Windows) and type the command as follows
```
ssh username@login2.hpc.brin.go.id
```
You'll be landed at the `trembesi02` node which is a **login** node.

## Introducing the module
There are several ways to run an application on MAHAMERU. First, you can install the app yourself by compiling it from the source code, or you can use Conda/Mamba to install your app and its environment, including the necessary dependencies. The latter option often reduces headaches. Secondly, you can use a software module that we provide on the server. All you need to do is load the module and start using it—no hassle.

To show modules available in the server, type
```
module avail
```
Or the shorter command one
```
ml av
```
`ml` means module `av` means available
Now, how to use the app. For instance, you want to use FastQC for QC-ing your reads, just type
```
module load bioinformatics/fastqc
```
or the using shorter command
```
ml bioinformatics/fastqc
```
Show listed module on your environment:
```
module list
```
or the shorter one
```
ml
```
If you see several modules loaded in your environment, don’t be confused; these are pre-loaded modules that automatically load when you log in.

## Two type of job submission
1. Interactive Job Submission
2. Non-interactive Job Submission
In this workshop, we will use both. Quality Control (QC) of the reads will be done in interactive mode, while the rest (assembly and annotation) will be done using a script in non-interactive mode.

WARNING: You are not allowed to run applications on the login node (trembesi02). If your application hogs the resources, the system will automatically terminate it. Therefore, you need to submit your job either in interactive or non-interactive mode. However, simple tasks such as using `wget`, `curl`, or unzipping files typically do not consume significant resources, so running them on the login node is acceptable.

Are you ready? Let's start!

## Preparing the samples
All samples we'll use are already prepared in a shared directory, so you don't need to download them. The things you need to do is just make a symbolic link to your prepared directory.

To make it organized, we'll create a directory called `workshop_microbial` under your home directory.
```
mkdir workshop_microbial
```
Enter the directory
```
cd workshop_microbial
```
Create a directory called `raw` to store the raw data. Do you know how to do that?
<details>
  <summary>Click here if you have no idea</summary>
  Symply type 'mkdir raw'
</details>

Create a symbolic link for all the raw samples we've prepared using `ln -s` command and store it under directory called 'raw'.
```
ln -s /mgpfs/data/workshop_raw/* raw/
```
Check the result by listing the raw directory using the `ls` command
```
ls raw
```
If succeeds, the directory will contain a symbolic link of raw data which are two Illumina reads (forward and reverse) and one ONT reads.
```
$ ls raw/
illumina_f.fq  illumina_r.fq  minion_2d.fq

# Do not copy this snippet code, it just shows you the result should appear.
```
All good? :)

## 1. Interactive job submission 
In this section, we will QC-ing our samples using two tools including FastQC and NanoPlot for Illumina reads and ONT reads, respectively. Don't worry, you don't need to install it because both tools are already available in the module. 

