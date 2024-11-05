# fetchngs
Workflow to fetch fastq files and metadata on the HPCC using SLURM.

## Steps
1. Create a directory for your your analysis (ex: /mnt/home/$username/rnaseq).
2. Make a samplesheet table in csv format (ex: ids.csv).
3. Make a nextflow configuration file (ex: nextflow.config).
4. Write a bash script to run the pipeline using SLURM (ex: run_fetchngs.sb).
5. Run the pipeline from your rnaseq directory (ex: sbatch run_rnaseq.sb).

# Create a directory for your your analysis
Login to your HPCC account using OnDemand. Navigate to your home directory by clicking 'Files' in the navigation bar. Select 'Home Directory'.

Create a directory for your analysis by clicking 'New Directory'. Name your directory (ex: rnaseq). Navigate to the newly created directory.

# Make a samplesheet table
In your rnaseq directory, click 'New File'. Name the file 'ids.csv'. Click the ⋮ symbol and select edit. Create the samplesheet table, for example:
```
SRX19731157
SRX19731158
SRX19731159
SRX19731160
SRX19731161
SRX19731162
SRX19731163
SRX19731164
SRX19731165
```
Save the ids.csv file and return to your directory.

# Make a nextflow configuration file
In your rnaseq directory, click New File. Name the file 'nextflow.config'. Click the ⋮ symbol and select edit. Create the Nextflow config file:
```
process {
    executor = 'slurm'
}
```
Save the nextflow.config file and return to your directory.

# Write a bash script to run the pipeline using SLURM
In your rnaseq directory, click New File. Name the file 'run_fetchngs.sb;. Write the bash script, using #SBATCH directives to set resources, for example:
```
#!/bin/bash

#SBATCH --job-name=$jobname_fetchngs
#SBATCH --time=4:00:00
#SBATCH --mem=24GB
#SBATCH --cpus-per-task=8

cd /mnt/home/$username/rnaseq
module load Nextflow/23.10.0

nextflow pull nf-core/fetchngs
nextflow run nf-core/fetchngs -r 1.12.0 --input ./ids.csv  -profile singularity --outdir ./fastq_files --nf_core_pipeline rnaseq --download_method sratools -work-dir /mnt/scratch/$username/fetchngs -c ./nextflow.config
```
Save the run_fetchngs.sb file and return to your directory.

# Run the pipeline from your directory
In your rnaseq directory, click Open in Terminal to enter a development node. Run jobs on the SLURM cluster:
```
sbatch run_fetchngs.sb
```
Check the status of your pipeline job:
```
squeue -u $username
```
Note: replace $username with your username
