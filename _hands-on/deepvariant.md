---
topic: genomics
title: Tutorial1 - WGS analysis with DeepVariant container 
---

## Analysis of whole genome sequencing (WGS) data using DeepVariant Singularity container
Run the [DeepVariant method](https://github.com/google/deepvariant) to perform variant calling on whole-genome (WGS) and whole-exome (WES) sequencing datasets within the LUMI supercomputing environment using a Singularity container.

This analysis requires the following input files:
   - A reference genome in[FASTA](https://en.wikipedia.org/wiki/FASTA_format) format, along with its corresponding index file (.fai)
   - An aligned reads file in [BAM](http://genome.sph.umich.edu/wiki/BAM) format, along with its corresponding index file (.bai)

For the purpose of this tutorial, sample test data will be provided via a downloadable link in later sections.

### Expected learning from tutorial:
Upon completion of this tutorial you will learn to: 
- Pull the Singularity image (e.g., DeepVariant) from a container registry.
- Deploy a singularity container for WGS analysis on LUMI as a batch job on Puhti

### Run WGS analysis with DeepVariant 

1. First login to LUMI supercomputer using *SSH*:
   ```bash
   ssh yourcscusername@lumii.csc.fi
   ```
> ‼️ In order to log in with SSH from the command-line, you must have also set up SSH keys and uploaded your public key to MyCSC

2. Navigate to your *scratch* directory and prepare a folder for analysis:
   ```bash
    cd /scratch/project_xxxx/$USER   # replace xxxx with your course (or own) project number
    mkdir deepvariant && cd deepvariant
   ```

3. Prepare Singularity image from docker image for DeepVariant analysis. Here we use   DeepVariant Docker image from google container registry with a
   specific tag (i.e., v1.5.0).  It is advisable to use LOCAL_SCRATCH for Singularity TMPDIR and CACHEDIR as below:

   ```bash
    export APPTAINER_TMPDIR=$LOCAL_SCRATCH
    export APPTAINER_CACHEDIR=$LOCAL_SCRATCH
    singularity build dv_150.sif docker://gcr.io/deepvariant-docker/deepvariant:1.5.0
    ```
   This image building process for DeepVariant takes sometime as it is a bigger image with several layers.

4. Download and unpack the test data for DeepVariant analysis as below:

   ```bash
    wget https://a3s.fi/containers-workflows/deepvariant_testdata.tar.gz
    tar -xavf deepvariant_testdata.tar.gz
   ```

5. Prepare a batch script (e.g., deepvariant_lumi.sh) to run WGS analysis on LUMI. A batch script template with all necessary information is provided below. You
  are required to use a valid project number in the script before submitting it to Puhti cluster.
   
   ```bash
   #!/bin/bash
   #SBATCH --account=project_465001676
   #SBATCH --partition=debug
   #SBATCH --time=00:30:00
   #SBATCH --nodes=1
   #SBATCH --ntasks-per-node=32
   #SBATCH --job-name=dv_toy

   export TMPDIR=$PWD
   export SINGULARITYENV_TMPDIR=$PWD
   export SINGULARITYENV_TMP=$PWD


   singularity -s exec  -B $PWD -B $PWD/testdata:/data \
   dv_cpu_150.sif \
   /opt/deepvariant/bin/run_deepvariant \
   --model_type=WGS \
   --ref=/data/ucsc.hg19.chr20.unittest.fasta \
   --reads=/data/NA12878_S1.chr20.10_10p1mb.bam  \
   --regions "chr20:10,000,000-10,010,000" \
   --output_vcf=$PWD/output.vcf.gz  \
   --output_gvcf=$PWD/output.g.vcf.gz
   ```

6. Submit your job to LUMI supercomputer
   
   ```bash
   sbatch -J deepvariant deepvariant_lumi.sh´
   ```
   If the analysis is completed successfully (**hint**: check the status of submitted job using `squeue -u $USER` command or using `seff <jobid>`), you are able
    to see the vcf files as output in the current directory.
