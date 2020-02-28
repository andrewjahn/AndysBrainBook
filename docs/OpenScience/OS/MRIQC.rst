.. _MRIQC:

=============
BIDS App Tutorial #1: MRIQC
=============

-------------

.. note::

  This article was contributed by `Daniel Levitas <https://perceptionandneuroimaging.psych.indiana.edu/people/daniellevitas.html>`__ of the Perception and Neuroimaging Lab at Indiana University.
  
What is MRIQC?
*************

MRIQC is a BIDS App that leverages BIDS compliant datasets in order to perform quality assessments (QA) on T1w, T2w, and/or functional MRI acqusitions. Assessments come in the form of handy visual reports that can be used to assess the quality of the collected data, and determine whether the quality is sufficient for processing and analysis. For a more in-depth (and better) overview of MRIQC, check out their homepage `here <https://mriqc.readthedocs.io/en/stable/>`__.

MRIQC Tutorial
**************

This tutorial will demonstrate how to install MRIQC and run it on a dataset. The data that we'll be using is the BIDS-ified output from the `BIDS Overview and Tutorial <https://andysbrainbook.readthedocs.io/en/latest/OpenScience/OS/BIDS_Overview.html>`__. If you haven't checked out the BIDS tutorial, or aren't familiar with BIDS conversion, I'd highly recommend completing that before getting started here. If however you fancy yourself a BIDS god (or goddess) and/or are looking to take the path of least resistance, you can download the BIDS output data `here <https://drive.google.com/drive/folders/13NmGGaRxqgSaqs8zUOGLxlcj1I6BrNle?usp=sharing>`__. Be sure to download the entire folder and not just the subfolders or files. If the download appears to have stalled, cancel the current download and retry. Once downloaded, extract/unzip the files (presumably from your Downloads folder), and type the following into the command line, line by line:

::

  mv ~/Downloads/BIDS_data $HOME/BIDS_tutorial
  
  
MRIQC Installation Option #1: Singularity
*******************************

MRIQC runs as a Docker or Singularity container, so we'll first need to build the container. If you're on a Linux system then you'll want to build a Singularity container because Docker containers are not permitted on HPC systems due to required root access permissions that no HPC admin is going to allow. If you're using MacOS or Windows, skip this and review the Docker installation section instead. If you are working on a university HPC, you may already have the Singularity software available (just make sure you have version >= 2.5). Once ready, type the following into the command line to build the container:

::

  singularity build $HOME/mriqc-0.15.1.simg docker://poldracklab/mriqc:0.15.1
  
MRIQC Installation Option #2: Docker
***************************

Go to the Docker `installation page <https://docs.docker.com/install/>`__ and select the download for your operating system. Then go through the steps to install Docker. Once finished, type the following into the command line to build the container. 

::

  docker run -it poldracklab/mriqc:0.15.1 --version
  

Making a script to run MRIQC
****************************

Running MRIQC entails specifying different command line options in order to properly run. Rather than doing this directly on the command line, we will make a script to run it. Firstly, tpye the following into the command line:

::

  touch $HOME/BIDS_tutorial/code/mriqc.sh
  
This creates a blank bash script file to run MRIQC. Below, I've provided a mock script that you can copy and paste into the mriqc.sh file. 

::

  #!/bin/bash

  #User inputs:
  bids_root_dir=$HOME/BIDS_tutorial
  subj=01
  nthreads=2
  mem=10 #gb
  container=docker #docker or singularity

  #Make mriqc directory and participant directory in derivatives folder
  if [ ! -d $bids_root_dir/derivatives/mriqc ]; then
  mkdir $bids_root_dir/derivatives/mriqc
  fi

  if [ ! -d $bids_root_dir/derivatives/mriqc/sub-${subj} ]; then
  mkdir $bids_root_dir/derivatives/mriqc/sub-${subj}
  fi

  #Run MRIQC
  echo ""
  echo "Running MRIQC on participant $s"
  echo ""

  if [ $container == singularity ]; then
    unset PYTHONPATH; singularity run $HOME/mriqc_0.15.1.simg \
    $bids_root_dir $bids_root_dir/derivatives/mriqc/sub-${subj} \
    participant \
    --n_proc $nthreads \
    --hmc-fsl \
    --correct-slice-timing \
    --mem_gb $mem \
    --float32 \
    --ants-nthreads $nthreads \
    -w $bids_root_dir/derivatives/mriqc/sub-${subj}
  else
    docker run -it --rm -v $bids_root_dir:/data:ro -v $bids_root_dir/derivatives/mriqc/sub-${subj}:/out 
    poldracklab/mriqc:0.15.1 /data /out participant \
    --n_proc $nthreads \
    --hmc-fsl \
    --correct-slice-timing \
    --mem_gb $mem \
    --float32 \
    --ants-nthreads $nthreads \
    -w $bids_root_dir/derivatives/mriqc/sub-${subj}
  fi
   

Before running, change the container variable to either *docker* or *singularity*, depending on which container you installed. To run the script type the following into the command line, line by line:

::

  bash
  source $HOME/BIDS_tutorial/code/mriqc.sh

MRIQC will take awhile to run to completion (**approximately 40 min**), so you can leave the terminal window aside until then. It's worth noting that the example MRIQC command is rather bare-bones; if you're interested in applying additional or different options to your MRIQC command, refer to them `here <https://mriqc.readthedocs.io/en/stable/running.html>`__. The time it takes MRIQC to finish is contingent on the size of your data, the amount of processing power you're feeding MRIQC, and the feature options selected, so running MRIQC on a different dataset with different options may result in a longer (or shorter) completition time. 

Assessing MRIQC QA Reports
**************************

To access the reports, go to the output directory by typing the following into the command line:

::

  cd $HOME/BIDS_tutorial/derivatives/mriqc/sub-01
  
MRIQC performs two analysis stages: participant and group. In a nutshell, the participant level analysis stage computes the various diagnoistics and visualizations per subject, and the group level merges the diagnostics. The group level reports can be easily identified by the "group" label in the file names. The participant reports are the other HTML files -- each T1w, T2w, and functional acqusition has an associated HTML report; you will need to use a browser to view them. If you are on an HPC, you may already have a browser installed. For example, mine contains Firefox, so in order to open the T1w HTML report via the command line I would type this:

::

  firefox $HOME/BIDS_tutorial/derivatives/mriqc/sub-01/sub-01_T1w.html


If you're working on a personal laptop or equivalent else then you can right-click on one of the reports and open it with your default browser. 

Regardless of which HTML report you open, you will quickly notice that there is **A LOT** of information provided. A lot. While parsing and trying to understand all the diagnostics can be daunting, there are several ones that I would recommend you absolutely check. If you're viewing any of the participant level reports, these can be found towards the bottom of the report in the *Extracted Image Quality Metrics (IQMs)* tab.

T1w amd T2w reports: Contrast-to-Noise Ratio (CNS)
task reports: motion parameters (fd mean, fd num, fd perc), and Signal-to-Noise Ratio (SNR)

For the plots in the functional reports, I'd highly recommend examining the *fMRI summary plot* to assess the motion across the functional acquisition period. 

The group HTML reports will take the values from the *Extracted Image Quality Metrics (IQMs)* in the participant HTML files and plot them together. This provides a wonderful visualization of your data, based on various diagnostics. Since we only have one participant, the group reports aren't particularily meaningful, but with a dataset set containing many subjects you can visually inspect for outliers. In addition to the HTML reports, there are also corresponding .tsv files that contain the diagnostics, which are tremendously useful for excluding data (e.g. specific subject runs) based on a-priori criteria. 

For additional information on the many diagnostics MRIQC provides, check out their documentation `here <https://mriqc.readthedocs.io/en/stable/measures.html>`__. 

Final Thoughts
**************

In this tutorial we went over how to set up and run MRIQC on a BIDS dataset containing one subject. The purpose was to become familiar with how to run the software and assess the QA reports. If you found this useful and would like to apply MRIQC to your own data, you may want to include additional features in the script. Since this tutorial was an extrememly simplified implentation of MRIQC, you may encounter issues when running it on your own data. Fear not, you can post your questions/issues on `NeuroStars <https://neurostars.org/>`__ or MRIQC's `github page <https://github.com/poldracklab/mriqc/issues>`__

Additional MRIQC links
**********************

If you've finished this tutorial and find yourself craving more, check out `Saren Seeley's BIDS, MRIQC, and fMRIPrep Tutorial <https://rpubs.com/sarenseeley/bids-fmriprep-mriqc>`__. 


