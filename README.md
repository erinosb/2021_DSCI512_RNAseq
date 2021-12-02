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
$ git clone <paste path to github repository here>
```

**Explore what you obtained.**

Notice that instead of having a single script, you now have a few scripts. These will work in a **Two step** method for executing jobs on summit. The `execute` script calls the `analyze` script. This Readme file and a license file were also downloaded.

Let's copy the two scripts up one directory. This will create duplicate copies for you to edit on and will move the scripts directly into ''02_scripts'', not its sub-directory.

```bash
$ cd 2020_DSCI512_RNAseq
$ cp RNAseq_analyzer_211202.sh ..
$ cp execute_RNAseq_pipeline.sbatch ..
$ cd ..
```
