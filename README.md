# Singularity Definition Files Repository
This repository contains Singularity definition files., i.e. recipes, for building Singularity images. To build the images, one needs to have [singularity installed](https://docs.sylabs.io/guides/3.0/user-guide/installation.html). 

# Cloning the Repository
To clone this repository, open your terminal and run the following command:

`git clone https://github.com/juliawiggeshoff/singularity-recipes.git`

# Building images

After cloning the repo:

1. Move to the directory

`cd singularity-recipes`
  
2. Choose the path and name of the image you want to create and one of the definition files from the repp

`singularity build [--fakeroot] /path/to/name_of_image.sif name_of_recipe.def`

# Singularity Recipes
## awscli-1.29.62.def

This Singularity image provides the Amazon Web Services Command Line Interface [(aws-cli), version 1.29.62,](https://pypi.org/project/awscli/1.29.62/) installed with pip. It offers a unified command line interface for interacting with various AWS services. 

The built image has an average size of 180 MB

## vcflib-1.09.def

The Singularity image built with this recipe contains [vcflib version 1.09, build version after commit 0a05bec](https://github.com/vcflib/vcflib), used to process VCF (Variant Calling Format) files. Running the tools inside of the singularity became particularly important as the bioconda recipe for vcflib has known issues since June 2023 (see https://github.com/vcflib/vcflib/issues/389), and it's a bit of a dependency nightmare when building from source. 

The built image has an average size of 520 MB. This is the only image made [available](https://github.com/juliawiggeshoff/singularity-recipes/blob/main/vcflib-1.09.simg) in the current repository, after being requested by another user. Overall, there shouldn't be issues when building the images. If that's not the case, please let me know :)

Tools can be executed directly, like:
```
vcfwave -L 1000 input.vcf | bgzip -f -c > output.vcf.gz
tabix -f -p vcf output.vcf.gz
```

## oncokb-annotator-3.4.0.def

This Singularity image is mainly designed for the [oncokb-annotator](https://github.com/oncokb/oncokb-annotator) version 3.4.0, a tool used for annotating cancer variants with information from the OncoKB database. Note that to use oncokb-annotator, you need an [API token](https://github.com/oncokb/oncokb-annotator#oncokb-api). The recipe includes the installation of several critical tools for genomic data processing:

- [Ensembl's VEP](https://github.com/Ensembl/ensembl-vep) (Variant Effect Predictor): This tool annotates variants from VCF files with information about their effects on genes, transcripts, proteins, and regulatory regions. Note that cache files are **NOT** installed as part of the build process, just the API. You need to either modify the recipe if you wish to OR mount a local directory with the cache 110.0 files when running vep/vcf2maf in the container.
- [vcf2maf](https://github.com/mskcc/vcf2maf): Converts VCF files to MAF (Mutation Annotation Format), which is a prerequisite for annotation with MafAnnotator.py from oncokb-annotator. Modifications were made to vcf2maf.pl to [rename STRAND_VEP to TRANSCRIPT_VEP](https://docs.gdc.cancer.gov/Data/File_Formats/MAF_Format/#notes-about-gdc-maf-implementation), avoiding confusion with the [Genomic] strand column, which is always +.
- [Samtools](https://github.com/samtools/samtools) & [Htslib](https://github.com/samtools/htslib): Tools and library for manipulating high-throughput sequencing data formats.

The built image has an average size of 620 MB. 

All of the necessary tools, vep, vcf2maf, oncokb-annotator, samtools, are found in the container inside of `/opt/`

## bamsurgeon.def

[BAMSurgeon](https://github.com/adamewing/bamsurgeon) is a tool designed for artificially introducing mutations into BAM files, creating a synthetic tumour file, which is useful for benchmarking variant calling tools and bioinformatics pipelines. The recipe is based on the original [Dockerfile](https://github.com/adamewing/bamsurgeon/blob/master/Dockerfile) from the tool's author, but the container did not work. Therefore, I slightly modified the recipe so that the container should work now.

The built image has an average size of 950 MB.

bamsurgeon is inside of `/opt/bamsurgeon`, following the directory structure from the cloned [repo](https://github.com/adamewing/bamsurgeon). The other executables needed to operate bamsurgeon are found in `/usr/local/bin`, found in $PATH. This can be verified by running `singularity exec bamsurgeon.img python3 -O /opt/bamsurgeon/scripts/check_dependencies.py`, which should return `All dependencies are satisfied`

