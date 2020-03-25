.. _fMRIPrep:

=============
BIDS App Tutorial #2: fMRIPrep
=============

-------------

.. note::

  This article was contributed by `Daniel Levitas <https://perceptionandneuroimaging.psych.indiana.edu/people/daniellevitas.html>`__ of the Perception and Neuroimaging Lab at Indiana University.
  
What is fMRIPrep?
*************

fMRIPrep is a BIDS App that leverage BIDS compliant dataset to perform standardized preprocessing of fMRI data. This pipeline utilizes the leading tools from different fMRI software packages (AFNI, FSL, ANTs, Freesurfer, nipype) to perform specific steps in the preprocessing. In addition, users can easily access the workflows to determine how their data were preprocssed, thus providing transparency and reproducibility. 

Within the neuroimaging community there exists a plethora of preprocessing pipelines for fMRI data -- most of which are not made publicly available. The result is that individual labs oftentimes have idiosyncratic preprocessing pipelines that can lead to issues of reproducibility.

fMRIPrep Tutorial
*****************

This tutorial will demonstrate how to install fMRIPrep and run it on a dataset. The data that we'll be using is the BIDS-ified output from the `BIDS Overview and Tutorial <https://andysbrainbook.readthedocs.io/en/latest/OpenScience/OS/BIDS_Overview.html>`__. 

If you haven't looked through and completed the previous tutorials in this OpenScience section the I'd recommend that. If however you just want to get started with this tutorial instead, you can download the BIDS output data `here <https://drive.google.com/drive/folders/11qNNVmD-T8OoZy9NFqHjcleWIcso6ZDI?usp=sharing>`__. Be sure to download the entire folder and not just the subfolders or files. If the download appears to have stalled, cancel the current download and retry. Once downloaded, extract/unzip the files if they haven't been already (presumably from your Downloads folder), and type the following into the terminal, line by line:

::

  mkdir $HOME/BIDS_tutorial
  mv ~/Downloads/BIDS_data/* $HOME/BIDS_tutorial
  rm -rf ~/Downloads/BIDS_data*

fMRIPrep Installation Option #1: Singularity
*******************************

fMRIPrep runs as a Docker or Singularity container, so we'll first need to build the container. If you're on a High Performance Computing (HPC) cluster then you'll want to build a Singularity container because Docker containers are not permitted on HPC systems due to required root access permissions that no HPC admin is going to allow. If you're using MacOS or Windows, skip this and review the Docker installation section instead. If you are working on a university HPC, you may already have the Singularity software available (just make sure you have version >= 2.5). Once ready, type the following into the terminal to build the container:

::

  singularity build $HOME/fmriprep-20.1.0rc1.simg docker://poldracklab/fmriprep:20.1.0rc1

fMRIPrep Installation Option #2: Docker
***************************

Assuming that you do not have Docker installed, go to the Docker `installation page <https://docs.docker.com/install/>`__ and select the download for your operating system. Once downloaded, click on the Docker.dmg installer and drag the Docker icon into your Applications (you may need your computer's admin password for this). **Be sure to click the Docker icon to open it**. At this point the docker command should now be in your $PATH, and you can type the following into the terminal to build the container: 

::

  docker run -it poldracklab/fmriprep:20.1.0rc1 --version
  
Be aware that this will likely take ~15 minutes.

As of the time of writing this tutorial (March 24, 2020), the most recent version of fMRIPrep is 20.1.0rc1, which is what we are using in this tutorial. Unlike MRIQC, fMRIPrep has more releases, normally for bug fixes and feature additions/modifications. Oftentimes, the changes from version to version are minor and do not require upgrading to the latest version, unless the changes in the newest version are pertinent to you. Regardless, **you should preprocess your dataset using the same fMRIPrep version**. 

Installing TemplateFlow
***********************
`TemplateFlow <https://github.com/templateflow>`__ is a repository containing various standardized templates to normalize your data to in fMRIPrep. If for example you have a pediatric dataset, there is a pediatric template in TemplateFlow at your disposal. 

If you do not have pip installed (or in your $PATH), refer back to the `BIDS Overview and Tutorial <https://andysbrainbook.readthedocs.io/en/latest/OpenScience/OS/BIDS_Overview.html>`__ for guidance. 

To install TemplateFlow, type the following into the terminal, line by line:

::

  mkdir $HOME/templateflow
  pip install templateflow --target $HOME/.cache
  unzip $HOME/.cache/templateflow/conf/templateflow-skel.zip -d $HOME/templateflow
  
Once finished, you should see multiple template options in the $HOME/templateflow folder.

Making a script to run fMRIPrep
*******************************

Running fMRIPrep entails specifying different command line options in order to properly run. Rather than doing this directly through the terminal, we will make a script to run it. Firstly, type the following into the terminal:

::

  touch $HOME/BIDS_tutorial/code/fmriprep.sh
  
This creates a blank bash script file to run fMRIPrep. Below, I've provided a mock script that you can copy and paste into the fmriprep.sh file. To do this you will first need to open the fmriprep.sh file, by typing the following into the terminal:

::

  vim $HOME/BIDS_tutorial/code/fmriprep.sh
  
Press the “i” key, and paste the contents below into the file. To save and close the file, press the Escape button, and type the following: :wq

::

  #!/bin/bash

  #User inputs:
  bids_root_dir=$HOME/BIDS_tutorial
  subj=01
  nthreads=2
  mem=10 #gb
  container=docker #docker or singularity

  #Begin:
  
  #Convert virtual memory from gb to mb
  mem=`echo "${mem//[!0-9]/}"` #remove gb at end
  mem_mb=`echo $(((mem*1000)-5000))` #reduce some memory for buffer space during pre-processing

  export TEMPLATEFLOW_HOME=$HOME/templateflow

  #Run fmriprep
  if [ $container == singularity ]; then
    unset PYTHONPATH; singularity run -B $HOME/templateflow:/opt/templateflow $HOME/fmriprep-20.1.0rc1.simg \
      $bids_root_dir $bids_root_dir/derivatives \
      participant \
      --skip-bids-validation \
      --md-only-boilerplate \
      --participant-label $subj \
      --fs-no-reconall \
      --output-spaces MNI152NLin2009cAsym:res-2 \
      --nthreads $nthreads \
      --stop-on-first-crash \
      --mem_mb $mem_mb \
      -w $bids_root_dir/derivatives
  else
    unset PYTHONPATH; docker run -ti --rm -v $bids_root_dir:/data:ro -v $bids_root_dir/derivatives:/out \
      poldracklab/fmriprep:20.1.0rc1 /data /out/out \
      participant \
      --skip-bids-validation \
      --md-only-boilerplate \
      --participant-label $subj \
      --fs-no-reconall \
      --output-spaces MNI152NLin2009cAsym:res-2 \
      --nthreads $nthreads \
      --stop-on-first-crash \
      --mem_mb $mem_mb \
      -w $bids_root_dir/derivatives
  fi
  
To ensure that the information was added and saved to the script, you cna type the following into the terminal:

::

  cat $HOME/BIDS_tutorial/code/fmriprep.sh
  
Before running, change the container variable in the script to either *docker* or *singularity*, depending on which container you installed. To run the script, type the following into the terminal, line by line:

::

  bash
  source $HOME/BIDS_tutorial/code/fmriprep.sh
  
fMRIPrep will take several hours to run.
