# Accelerating Microbial Insight with HPC and Bioinformatics

This instruction provides an example of analysing microbial data using MAHAMERU BRIN HPC. In this workshop, all the software that will be used is already available as a module, hence you don't need to prepare the environment.

## Login to MAHAMERU
Open your terminal (Linux, MacOS) or PowerShell (Windows) and type command as following
```
ssh username@login2.hpc.brin.go.id
```
You'll be landed at `trembesi02` node which is a **login** node.

## Two type of job submission
1. Interactive Job Submission
2. Non-interactive Job Submission
In this workshop, we will use both. Quality Control (QC) of the reads will be done in interactive mode, while the rest (assembly and annotation) will be done using a script in non-interactive mode.

Are you ready? Let's start

## Preparing the samples
All samples we'll use are already prepared in a shared directory.
