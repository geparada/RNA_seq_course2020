# Install #

Setup [miniconda](https://docs.conda.io/en/latest/miniconda.html) and then update conda: 

    conda update -n base -c defaults conda

Add channels

    conda config --add channels defaults
    conda config --add channels conda-forge
    conda config --add channels bioconda

Clone the course repository:

    git clone https://github.com/geparada/RNA_seq_course2020

Create a conda enviroment with snakemake installed on it. We can call this virtual enviroment `snakemake_env`, but any other name would also work.

    conda create -n snakemake_env snakemake   

Activate conda enviroment:
    
    conda activate snakemake_env

# Download Data #

    snakemake --resources get_data=1 --use-conda download_all

# Run #

In order to check that the full snakemake pipeline is working correctly, we can try with an dry-run (`-n`) while we print the commands of every step (`-p`):

    snakemake --use-conda -np whippet_delta

To run the full you need to download D. melanogaster transcript annotation from UCSC Table Browser and save it as `Gene_annotation/dm6.Ensembl.genes.gtf` (replacing the empty file from this repository). Then select the number of processor you want to run snakemake with and run it as:

    snakemake --use-conda whippet_delta
    

    
# Run from a cluster #

We provide a `cluster.json` where we provide the variables that `lsf` (current queueing system at Wellcome Sanger Institute cluster) needs to run. You can create a new `cluster.json`file to addapt this `Snakefile` to any other queueing system.

    snakemake --cluster-config cluster.json --cluster "bsub -n {cluster.nCPUs} -R {cluster.resources} -c {cluster.tCPU} -G {cluster.Group} -q {cluster.queue} -o {cluster.output} -e {cluster.error} -M {cluster.memory}" --use-conda -k -j 100 
