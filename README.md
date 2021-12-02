# 2021_DSCI512_RNAseq
course pipeline for DSCI512; Fall 2021


-----


*DSCI512 - RNA sequencing data analysis - course scripts*

*A simple set of wrappers and tools for RNA-seq analysis. These tools were designed for the DSCI512 RNA-seq analysis class at Colorado State University*

Below is a tutorial on the use of these scripts:

----


## Let's download the script templates I've written on github.

We will build on these scripts each class session.
You will be able to tailor these templates to your own purposes for future use and for the final project.


**Exercise**

  * Locate the green **code** button on the top right of this page. Click it.
  * Click on the clipboard icon. This will save a github URL address to your clipboard.
  * Switch over to JupyterHub linked to SUMMIT.
  * Navigate into your directory for `PROJ01_GomezOrte/02_scripts` and use `git clone` as shown below to pull the information from github to your location on SUMMIT.
  
```bash
$ cd /scratch/summit/<eID>@colostate.edu    #Replace <eID> with your EID
$ cd PROJ01_GomezOrte
$ cd 02_scripts
$ git clone <paste path to github repository here>
```

**Explore what you obtained.**

Notice that instead of having a single script, you now have a few scripts. These will work in a **Two step** method for executing jobs on summit. The `execute` script calls the `analyze` script. This Readme file and a license file were also downloaded.

Let's copy the two scripts up one directory. This will create duplicate copies for you to edit on and will move the scripts directly into ''02_scripts'', not its sub-directory.

```bash
$ cd 2021_DSCI512_RNAseq
$ cp RNAseq_analyzer_211202.sh ..
$ cp execute_RNAseq_pipeline.sbatch ..
$ cd ..
```

----
## Let's explore the RNAseqAnalyzer Script 


The **RNAseq_analyzer_211202.sh** script contains our pipeline. 

Let's briefly peek into it and see that it contains. 
  * Open **RNAseq_analyzer_211202.sh** in an editor window. You'll notice the following sections.

**The pipeline**
  * A shebang
  * A long comment section with documentation on its use
  * MODIFY THIS SECTION - *you will tailor this section to each job*
  * BEGIN CODE - *the code starts and reports how it is running*
  * META DATA - *this part pulls information out of the metadata file to create bash arrays*
  * PIPELINE - *right now this contains a for loop that will execute fastp. We will add onto this section each class*
  * VERSIONS - *this prints out the versions of software used for your future methods section*

The way this script works, we (as the user) modify the MODIFY THIS SECTION part, then when we run the script, we give it a metadata file as its first argument. We can execute the analyzer script like so...

```bash
$ bash RNAseq_analyzer_211202.sh ../01_input/metadatafile.txt
```

This will take a metadata file as input and loop over the content within that metadata file. It will pull the names of the .fastq file names to process from the first and second column of the metadata file and start processing them one at a time. It will name them by the 'short nickname' in the third column.

Now, we COULD execute the RNA_seq analyzer pipeline that way, but there is a problem with that. It would fail to use slurm, so we would overload the system. Instead, we need to execute this script using sbatch. We'll do that using a short mini-script called the execute program.

----
## Let's explore the Execute script 

The **execute_RNAseq_pipeline.sbatch** script will be used to submit the analyze script to the **job batch manager** called **SLURM**. This will put your analyze script in the queue and specify how it should be run on the supercomputer system.

For more background on SLURM:
  * [JOB SUBMISSIONS ON SUMMIT](https://curc.readthedocs.io/en/latest/running-jobs/batch-jobs.html)
  * [SLURM ON SUMMIT - FAQ](https://curc.readthedocs.io/en/latest/faq.html)
  * [SLURM DOCUMENTATION](https://slurm.schedmd.com/sbatch.html)

To execute the bash script, we will do the following...

```bash
$ sbatch execute_RNAseq_pipeline.sbatch
```

By doing this, the **execute** script will submit the **analyzer** script to **SLURM**. This will ensure the **analyzer** script is run at the proper time and with the requested resources on compute nodes on the SUMMIT system. What is SLURM? Slurm is a job scheduling system for large and small Linux clusters. It puts your job into a 'queue'. When the resources you have requested are available, your job will begin. SLURM is organized so that different users have different levels of priority in the queue. On SUMMIT, users who use fewer resources have higher priority. Power users have less priority and are encouraged to purchase greater access to the system if it is a problem.

Let's open **execute_RNAseq_pipeline.sbatch** in an editor window and explore how it works. 

``bash
#!/usr/bin/env bash

#SBATCH --job-name=RNAseq_pipeline 
#SBATCH --nodes=1                           # this script is designed to run on one node
#SBATCH --ntasks=2                          # modify this number to reflect how many cores you want to use (up to 24)
#SBATCH --partition=shas                    # modify this to reflect which queue you want to use. Either 'shas' or 'shas-testing'
#SBATCH --qos=normal                        # Should be 'normal' if partition is 'shas'; should be 'testing' if partition is 'shas-testing'
#SBATCH --mail-type=END                     # Keep these two lines of code if you want an e-mail sent to you when it is complete.
#SBATCH --mail-user=<youremail>             # add your e-mail here
#SBATCH --output=log_RNAseq_pipe_%j.txt     # this will capture all output in a logfile with %j as the job #

######### Instructions ###########

# Modify your SLURM entries above to fit your choices
# Below, modify the SECOND argument to point to YOUR metadata.file
# Below, you don't need to change $SLURM_NTASKS. It will automatically populate whatever you put in --ntasks=# above.
# Execute this script using $ sbatch execute_RNAseq_pipeline.sbatch



## Execute the RNA-seq_pipeline to run the pipeline
bash RNAseq_analyzer_211202.sh <metadatafile>  $SLURM_NTASKS


## Execute the cleanup script to zip .fastq files and delete extra files
#bash RNAseq_cleanup_211202.sh <metadatafile> 

```

  * This script is going to request 2 ntasks (threads) on 1 node. NOTE - never split a node. 
  * Notice that this script only has 1 line of code that will execute. The second bash line is commented out for now. 
  * The way you will use this script is by modifying the SLURM prepended commands to fit how you want the job to run.
  * Next, you will add in your <metadatafile> information. Mine will be ../01_input/tester_metadatafile.txt
  * Then, you will execute the script like so...
  
```bash
$ sbatch execute_RNAseq_pipeline.sbatch
```



