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

## Preparing the samples
All samples we'll use are already prepared in a shared directory, so you don't need to download them. The things you need to do is just make a symbolic link to your prepared directory.

To make it organized, we'll create a directory called `workshop_microbial` under your home directory.
```
mkdir workshop_microbial
```

