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

`singularity build /path/to/name_of_image.sif name_of_recipe.def`

# Singularity Recipes
## awscli-1.29.62.def

This Singularity image provides the Amazon Web Services Command Line Interface [(aws-cli), version 1.29.62,](https://pypi.org/project/awscli/1.29.62/) installed with pip. It offers a unified command line interface for interacting with various AWS services. 

The built image has an average size of 180 MB

## vcflib-1.09.de

The Singularity image built with this recipe contains [vcflib version 1.09, build version after commit 0a05bec](https://github.com/vcflib/vcflib), used to process VCF (Variant Calling Format) files. Running the tools inside of the singularity became particularly important as the bioconda recipe for vcflib has known issues since June 2023 (see https://github.com/vcflib/vcflib/issues/389), and it's a bit of a dependency-nightmare when building from source. 

The built image has an average size of 520 MB.
