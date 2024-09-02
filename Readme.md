# Accelerating Microbial Insight with HPC and Bioinformatics

This guide provides an example of analyzing microbial data using the MAHAMERU BRIN HPC. In this workshop, all the required software is already available as modules, so you don’t need to set up the environment. You can copy the code snippets on this page by clicking the top right corner of each code box and pasting them into your terminal.

## Login to MAHAMERU
Open your terminal (Linux, MacOS) or PowerShell (Windows) and type the folowing commands
```
ssh username@login2.hpc.brin.go.id
```
You'll be placed on the `trembesi02` node which is a **login** node.

## Introducing the module
There are several ways to run an application on MAHAMERU. First, you can install the app yourself by compiling it from the source code, or you can use Conda/Mamba to install your app and its environment, including the necessary dependencies. The latter option often reduces headaches. Secondly, you can use a software module that we provide on the server. All you need to do is load the module and start using it—no hassle.

To display the available modules on the server, type:
```
module avail
```
Same command:
```
ml av
```
`ml` means module `av` means available
Now, here’s how to use the application. For instance, if you want to use FastQC for quality checking your reads, just type:
```
module load bioinformatics/fastqc
```
or the using shorter command
```
ml bioinformatics/fastqc
```
Display loaded module on your environment:
```
module list
```
or use the shorter command:
```
ml
```
If you see several modules loaded in your environment, don’t be confused; these are pre-loaded modules that automatically load when you log in.

## The partition in SLURM
In SLURM, a partition is a set of compute nodes grouped together for scheduling jobs.
To see what's partition available in our cluster, simply type
```
sinfo
```
Will be look like this:

![Screenshot 2024-09-02 at 15 31 39](https://github.com/user-attachments/assets/50cb41e1-76b5-4785-b45d-3ff9fc9ce614)



## Two type of job submission
1. Interactive Job Submission
2. Non-interactive Job Submission
   
In this workshop, we will use both modes. Quality control (QC) of the reads will be performed in interactive mode, while the rest of the tasks (assembly and annotation) will be handled using a script in non-interactive mode.

NOTE: You are not allowed to run applications on the login node (trembesi02). If your application hogs the resources, the system will automatically terminate it. Therefore, you need to submit your job either in interactive or non-interactive mode. However, simple tasks such as using `wget`, `curl`, or unzipping files typically do not consume significant resources, so running them on the login node is acceptable.

Are you ready? Let's dive in!

## Preparing the samples
All samples we'll use are already prepared in a shared directory, so you don't need to download them.  You only need to create a symbolic link to this directory in your workspace.

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
   <pre>mkdir raw</pre>
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

# Do not copy this code snippet; it is just to show you the expected result.
```
All good? :)

Nice! let's keep going.

## 1. Interactive job submission 
In this section, we will perform quality control (QC) on our samples using two tools: FastQC for Illumina reads and NanoPlot for ONT reads. Don’t worry, you don’t need to install these tools, as they are already available in the module.

To start, we use `srun` command to launch the mode:
```
srun --partition=short --cpus-per-tasks=64 --pty bash
```
Here is the explanation:
- `srun`: command to start interactive mode
- `--partition`: This argument specifies the partition, in this case we use a partition called  'short'
- `--cpus-per-tasks`:This argument specifies the number of CPU cores to allocate for a task. For example, setting this to 64 would use all the cores on a single node.
- `--pty bash`: This argument is used to launch a bash shell on the compute node.
If the command succeeds, you’ll be placed on a compute node named trembesiXX (not trembesi02, which is the login node).

Make sure you're in the directory you created for this workshop, such as workshop_microbial. To confirm you're in the correct directory, use the `pwd` command. This command will print the current directory path. If you are in the wrong directory, use `cd` command to change to the correct working directory.

### 1.a QC Illumina reads
To do this, we use FastQC, one of the most popular tools for assessing the quality of sequence data. Remember, we are now working on an HPC node, which has more CPUs and memory compared to our laptop and supports a parallel environment. Not all applications have parallel execution capabilities, luckily FastQC supports multi-threading, which will make the job even faster.

Let's start with load the module, remember how to do this?
```
ml bioinformatics/fastqc
```
Run the app to asses our sequence data
```
mkdir fastqc_out  #Create a directory to store FastQC result
fastqc raw/illumina_*.fq -o fastqc_out -t 128
```
It will take a while to complete. If succeeds, expected output will be stored in 'fastqc_output' directory. Use `ls` command to see what's inside.
```
ls fastqc_output
```
FastQC will produce a report in HTML format that can be viewed using your preferred web browser. You can download the file using an SFTP client such as FileZilla, CyberDuck, or similar.
Note: use `fastqc --help` for more information
### 1.b QC ONT reads
Using FastQC to assess ONT reads is not suitable, as ONT uses a different Q-score. NanoPlot is the appropriate choice for this task.

First, load the module. Remember how to do this? 

Hint: The module name is 'bioinformatics/nanoplot'.
<details>
  <summary>Here is how</summary>
Oops, sorry about that. Please try it yourself.
</details>

Make sure the module is loaded, and then run the app:
```
NanoPlot -t 128 --fastq raw/minion_2d.fq -o nanoplot_out
```
It will take a while to complete.

More information provided in the help by executing `NanoPlot --help`. NanoPlot also creates a report in HTML format, stored in the output directory. To preview the report, download it and open it using a web browser.

Need some coffee? You can grab a cup while waiting for NanoPlot to finish.
<details>
  <summary>Enjoy coffee</summary>
  <pre>
                      (
                        )     (
                 ___...(-------)-....___
             .-""       )    (          ""-.
       .-'``'|-._             )         _.-|
      /  .--.|   `""---...........---""`   |
     /  /    |                             |
     |  |    |                             |
      \  \   |                             |
       `\ `\ |                             |
         `\ `|                             |
         _/ /\                             /
        (__/  \                           /
     _..---""` \                         /`""---.._
  .-'           \                       /          '-.
 :               `-.__             __.-'              :
 :                  ) ""---...---"" (                 :
  '._               `"--...___...--"`              _.'
jgs \""--..__                              __..--""/
     '._     """----.....______.....----"""     _.'
        `""--..,,_____            _____,,..--""`
                      `"""----"""`
  </pre>
</details>


### 1.c Use MultiQC to wrap all the QC result
MultiQC is a tool used to aggregate and visualize the results of various quality control (QC) tools into a single, comprehensive report.

Load the module
```
ml multiqc
```
Run the app
```
multiqc . -o multiqc_out
```
Expected result stored in the argument's value of `-o`.
HTML is a preferred format for QC software reports due to its dynamic and interactive features, such as those found in MultiQC outputs. 

## 2. Non-interactive job submission
In this mode, we will perform assembly and annotation using Unicycler and Prokka, respectively. To do this, we need to prepare a SLURM script for submission. Generally, this script will include several components, such as SLURM standard parameters, bash commands to navigate directories and access files, bash commands to load and execute modules or applications, and any other necessary commands. This process is similar to what we did in interactive mode, but the commands are written in a script file and executed non-interactively.

Create a `.slurm` file, e.g. `assembly.slurm`. There are many text editor available, such as `vim` and `nano`, for creating a file. In this case, we'll use `nano` for simplicity.
```
nano assembly.slurm
```
Copy and paste the code below into the nano editor

```
#!/bin/bash

#SBATCH --job-name=assembly_microbial
#SBATCH --ntasks=1
#SBATCH --partition=short
#SBATCH --cpus-per-task=64

# Your code goes here

# Print timestamp
echo -e "Job started at "
date

# Load all modules needed
ml bioinformatics/unicycler-env/1
ml bioinformatics/quast
ml bioinformatics/prokka-env/1

# De novo assembly using Unicycler, an assembly pipeline for bacterial genomes.
unicycler -t 128 -1 raw/illumina_f.fq -2 raw/illumina_r.fq -o unicycler_out

# Check the result using QUAST
quast.py -t 128 unicycler_out/assembly.fasta -o quast_out

# Annotate using Prokka
prokka --cpus 128 unicycler_out/assembly.fasta -o prokka_out

# Print timestamp
echo -e "Job finished at "
date

```
Save and close using keyboard stroke: 
- Ctrl + X 
- Press 'Y'
- Press Enter/Return to exit

Submit the job using `sbatch` command
```
sbatch assembly.slurm
```

Check job status using `squeue` command
```
squeue -u `whoami`
```
If everything works flawlessly, the status (ST) should be 'R'. This job may take some time to complete, but you can close the terminal, and the job will continue running

---


Awesome work! You've just hacked the microbial genome analysis on MAHAMERU HPC—now go out there and let those microbes know who's boss!

Coffee?   

![pngwing com](https://github.com/user-attachments/assets/34d7d746-c80a-43c2-8bd5-8b0de5d2a346)

