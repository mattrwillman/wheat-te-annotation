# wheat-te-annotation

This pipeline uses repeatmasker, repeatmodeler, and the TREP database  (v. 2019, https://trep-db.uzh.ch/) to annotate and mask transposable elements (TEs) in a wheat genome assembly.

# Workflow

1. Install software into a conda environment.

```
conda create --name te_env
source activate te_env
conda install snakemake seqkit repeatmodeler repeatmasker 
```

2. Clone the repository and edit config.yaml and snakemake_batch.sh files to fit your job.

3. Run [snakemake](https://snakemake.readthedocs.io/en/stable/).

Use batch script to submit job to SLURM. See example script below.

```
sbatch snakemake.slurm
```

Or, run snakemake locally.

```
snakemake [--cores N] --configfile config/config.yml
```

### SLURM file

Example SLURM file named "snakemake.slurm":

```
#!/bin/bash
#SBATCH --job-name="te_annotation"		#name of the job submitted
#SBATCH -p nodeName			#enter name of the queue you are submitting job to
#SBATCH -A project			#enter your project neame
#SBATCH -N 1				#number of nodes in this job
#SBATCH -n 48				#number of cores/tasks in this job
#SBATCH -t 14-00:00:00			#time allocated for this job hours:mins:seconds
#SBATCH --mail-user=emailAddress	#enter your email address
#SBATCH --mail-type=BEGIN,END,FAIL	#you will receive an email when job starts, ends, or fails
#SBATCH -o "stdout.%x.%j.%N"		#standard output, %x.%j.%N adds job name.number.node to outputfile name
#SBATCH -e "stderr.%x.%j.%N"		#standard error

# Module load miniconda
module load miniconda3

# Activate the correct environment
source activate te_env

# Perform the analysis with a pointer to the config file
snakemake --cores 'all' --configfile config/config.yml
```