# Accelerating Microbial Insight with HPC and Bioinformatics

This instruction provides an example of analysing microbial data using MAHAMERU BRIN HPC. In this workshop, all the software that will be used is already available as a module, hence you don't need to prepare the environment. The snippet code on this page is designed to be copied by clicking on the right corner of each code box and pasting it in your terminal.

## Login to MAHAMERU
Open your terminal (Linux, MacOS) or PowerShell (Windows) and type the command as follows
```
ssh username@login2.hpc.brin.go.id
```
You'll be landed at the `trembesi02` node which is a **login** node.

## Two type of job submission
1. Interactive Job Submission
2. Non-interactive Job Submission
In this workshop, we will use both. Quality Control (QC) of the reads will be done in interactive mode, while the rest (assembly and annotation) will be done using a script in non-interactive mode.

Are you ready? Let's start!

## 1. Preparing the samples
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
