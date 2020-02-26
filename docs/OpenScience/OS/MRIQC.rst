.. _MRIQC:

=============
BIDS App Part 1: MRIQC tutorial
=============

-------------

.. note::

  This article was contributed by `Daniel Levitas <https://perceptionandneuroimaging.psych.indiana.edu/people/daniellevitas.html>`__ of the Perception and Neuroimaging Lab at Indiana University.
  
What is MRIQC?
*************

MRIQC is a BIDS App that leverages BIDS compliant datasets in order to perform quality assessments (QA) on T1w, T2w, and/or functional MRI acqusitions. Assessments come in the form of handy visual reports that can be used to assess the quality of the collected data, and determine whether the quality is sufficient for processing and analysis. For a more in-depth (and better) overview of MRIQC, check out their homepage `here <https://mriqc.readthedocs.io/en/stable/>`__.

MRIQC Tutorial
**************

This tutorial will demonstrate how to install MRIQC and run it on a dataset. The data that we'll be using is the BIDS-ified output from the `BIDS Overview and Tutorial <https://andysbrainbook.readthedocs.io/en/latest/OpenScience/OS/BIDS_Overview.html>`__ page. If you haven't checked out the BIDS tutorial, or aren't familiar with BIDS conversion, I'd highly recommend completing that before starting this one. If however you fancy yourself a BIDS god (or goddess) and/or are looking to take the path of least resistance, you can download the BIDS output data `here <https://drive.google.com/drive/folders/13NmGGaRxqgSaqs8zUOGLxlcj1I6BrNle?usp=sharing>`__. Be sure to download the entire folder and not just the subfolders or files. If the download appears to have stalled, cancel the current download and retry. Once downloaded, extract/unzip the files (in your Downloads folder), and type the following into the command line, line by line:

::

  mv ~/Downloads/BIDS_data $HOME/BIDS_tutorial
  
  
MRIQC Installation
******************

MRIQC runs as a Docker or Singularity container, so we'll first need to build the container. For this tutorial we will be building a Singularity container because Docker containers are not permitted on HPC systems due to required root access permissions that no HPC admin is going to allow. If you are working on a university HPC, you may already have the Singularity software available (just make sure you have version >= 2.5). Once ready, type the following into the command line to build the container:

::

  singularity build $HOME/mriqc-0.15.1.simg docker://poldracklab/mriqc:0.15.1
  

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
  nthreads=1
  mem=20gb
  
  #Begin:
  for s in ${subj[*]}
  do
    echo ""
    echo $s
    echo ""

    #Make mriqc directory and participant directory in derivatives folder
    if [ ! -d $bids_root_dir/derivatives/mriqc ]; then
      mkdir $bids_root_dir/derivatives/mriqc
    fi

    if [ ! -d $bids_root_dir/derivatives/mriqc/sub-${s} ]; then
      mkdir $bids_root_dir/derivatives/mriqc/sub-${s}
    fi

    #Run MRIQC
    echo ""
    echo "Running mriqc on participant $s"
    echo ""
    unset PYTHONPATH; singularity run $HOME/mriqc_0.15.1.simg \
    $bids_root_dir $bids_root_dir/derivatives/mriqc/sub-${s} \
    participant \
    --participant-label $s \
    --n_proc $nthreads \
    --hmc-fsl \
    --correct-slice-timing \
    --mem_gb $mem \
    --float32 \
    --dry-run \
    -w $bids_root_dir/derivatives/mriqc/sub-${s}
  done


It's worth noting that the MRIQC command is rather bare-bones; if you're interested in applying additional or differnt features to your MRIQC command, refer to the options `here <https://mriqc.readthedocs.io/en/stable/running.html>`__.

