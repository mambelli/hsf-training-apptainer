---
title: "Bonus Episode"
teaching: 40
exercises: 0
questions:
- How to build singularity container for python packages?
- How to share singularity images?
objectives:
- To be able to build a singularity container and share it via GitHub packages
keypoints:
-  Python packages can be installed in singularity images along with ubuntu packages.
-  It is possible to publish and share singularity images over github packages.
---

> ## Prerequisites
> For this lesson, you will need,
> * Knowledge of Git [SW Carpentry Git-Novice Lesson](https://swcarpentry.github.io/git-novice/)
> * Knowledge of GitHub CI/CD [HSF Github CI/CD Lesson](https://github.com/hsf-training/hsf-training-cicd-github)
{: .prereq}

## Singularity Container for python packages

Python packages can be installed using a Singularity image. The following example illustrates how to write a definition file for building an image containing python packages.

```text
BootStrap: docker
From: ubuntu:20.04

%post
    apt-get update -y
    apt-get install wget -y
    export DEBIAN_FRONTEND=noninteractive
    apt-get install dpkg-dev cmake g++ gcc binutils libx11-dev libxpm-dev \
    libxft-dev libxext-dev python3 libssl-dev libgsl0-dev libtiff-dev \
    python3-pip -y

%post
    pip3 install numpy
    pip3 install awkward
    pip3 install matplotlib
    pip3 install zfit

%post
    pip3 install uproot4
    pip3 install particle
    pip3 install hepunits
    pip3 install scikit-hep-testdata

%post
    pip3 install hist
    pip3 install boost-histogram
    pip3 install vector
    pip3 install fastjet
    pip3 install iminuit
```
(This script is taken from [GitHub Repo Link](https://github.com/amanmdesai/singularity-scikit-hep)).


As we see, several packages are installed and this installation is done over several layers.


> ## Bonus question
> Why is installation over several layers advantageous?
>> ## Solution:
>> The reason is as follows:
>> - Building images is faster
>> - Pulling and pushing images is more efficient
> {: .solution}
{: .challenge}



## Publish Singularity images with GitHub Packages and share them!

It is possible to publish singularity images with GitHub packages. To do so, one needs to use GitHub CI/CD. A step-by-step guide is presented here.

* **Step 1**: Create a GitHub repository and clone it locally.
* **Step 2**: In the empty repository, make a folder called `.github/workflows`. In this folder we will store the file containing the YAML script for GitHub action, named `singularity-build-deploy.yml` (name doesn't really matter).
* **Step 3**: In the top directory of your github repo, create a file named `Singularity`.
* **Step 4**: Copy-paste the content above and add to the Singularity file. (In principle it is possible to build this image locally, but we will not do that here, as we wish to build it with GitHub CI/CD).
* **Step 5**: In the `singularity-build-deploy.yml` file, add the following content:

```text
name: Singularity Build Deploy

on:
  pull_request:
  push:
    branches: master

jobs:
  build-test-container:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    container:
        image: quay.io/singularity/singularity:v3.8.1
        options: --privileged

    name: Build Container
    steps:

      - name: Check out code for the container builds
        uses: actions/checkout@v2

      - name: Build Container
        run: |
         sudo -E singularity build container.sif Singularity

      - name: Login and Deploy Container
        run: |
           echo ${{ secrets.GITHUB_TOKEN }} | singularity remote login -u ${{ secrets.GHCR_USERNAME }} --password-stdin oras://ghcr.io
           singularity push container.sif oras://ghcr.io/${GITHUB_REPOSITORY}:${tag}
```

The above script is designed to build and publish a Singularity image with GitHub packages.
(The source code can be found here: [Link](https://github.com/amanmdesai/hello-world-singularity)).

* **Step 6**: Optionally add LICENSE and README, and then the repository is good to go.