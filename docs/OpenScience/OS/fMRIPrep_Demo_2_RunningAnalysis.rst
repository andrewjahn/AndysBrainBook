.. _fMRIPrep_Demo_2_RunningAnalysis:

==========================================
fMRIPrep Tutorial #2: Running the Analysis
==========================================

----------

Background
**********

This chapter closely follows the steps written in :ref:`Daniel Levitas's tutorial on fMRIPrep <fMRIPrep>`, which provides the background on what fMRIPrep is and how to install it. We will be following the second option, which is to use fMRIPrep through `Docker <https://docs.docker.com/get-docker/>`__. Once you have installed the appropriate version for your operating system, you also need to register on the FreeSurfer website `here <https://surfer.nmr.mgh.harvard.edu/registration.html>`__ and download the ``license.txt`` file. When it has been downloaded, move it to the ``derivatives`` folder of the ``Flanker`` directory by typing:

::

  mv ~/Downloads/license.txt ~/Desktop/Flanker/derivatives
  
Once you have all of these elements, you are ready to run fMRIPrep.


The Docker App
**************

The ``Docker`` App allows you to download a package of software programs that are needed to analyze a dataset. This package of programs is called a ``container``. For fMRI data, for example, we can use the terminal to upgrade the docker container for fmriprep:

::

  python -m pip install --user --upgrade fmriprep-docker
  
Which will install all of the programs used by fMRIPrep - for example, tools from software packages such as FSL and ANTs to assist with normalization and denoising the data. Before executing the code that will perform fMRIPrep on the data, you need to have Docker running.

.. note::

  If you try running the command above, you may get the following error: ``ImportError: cannot import name md5``. This can happen sometimes with Python version 2.7; to fix this error, install a `more recent version of Python <https://www.python.org/downloads/>`__, and then rerun the command:
  
  python3 -m pip install --user --upgrade fmriprep-docker

Contents of the fMRIPrep Script
*******************************

To run fMRIPrep, download the code from `this github page <https://github.com/andrewjahn/OpenScience_Scripts/blob/master/fmriprep.sh>`__. Then, navigate to your ``Flanker`` directory and create a new sub-directory by typing ``mkdir code``. We will place the script in this folder to keep our files organized:

::

  mv ~/Downloads/fmriprep.sh code
  
Let's take a look at what the code does by typing ``cat code/fmriprep_singleSubj.sh``. You should see something like this:

::

  #User inputs:
  bids_root_dir=$HOME/Desktop/Flanker
  subj=08
  nthreads=4
  mem=20 #gb
  container=docker #docker or singularity

  #Begin:

  #Convert virtual memory from gb to mb
  mem=`echo "${mem//[!0-9]/}"` #remove gb at end
  mem_mb=`echo $(((mem*1000)-5000))` #reduce some memory for buffer space during pre-processing

  export FS_LICENSE=$HOME/Desktop/Flanker/derivatives/license.txt

  #Run fmriprep
  if [ $container == singularity ]; then
    unset PYTHONPATH; singularity run -B $HOME/.cache/templateflow:/opt/templateflow $HOME/fmriprep.simg \
      $bids_root_dir $bids_root_dir/derivatives \
      participant \
      --participant-label $subj \
      --skip-bids-validation \
      --md-only-boilerplate \
      --fs-license-file $HOME/Desktop/Flanker/derivatives/license.txt \
      --fs-no-reconall \
      --output-spaces MNI152NLin2009cAsym:res-2 \
      --nthreads $nthreads \
      --stop-on-first-crash \
      --mem_mb $mem_mb \
      -w $HOME
  else
    fmriprep-docker $bids_root_dir $bids_root_dir/derivatives \
      participant \
      --participant-label $subj \
      --skip-bids-validation \
      --md-only-boilerplate \
      --fs-license-file $HOME/Desktop/Flanker/derivatives/license.txt \
      --fs-no-reconall \
      --output-spaces MNI152NLin2009cAsym:res-2 \
      --nthreads $nthreads \
      --stop-on-first-crash \
      --mem_mb $mem_mb \
      -w $HOME
  fi

The first block of code, "User Inputs", sets the path to where the data is, as well as which subject to analyze. ``nthreads`` specifies the number of processors to use, and ``mem`` specifies the amount of memory to use, in gigabytes. The variable ``container`` can be set to either ``docker`` or ``singularity``; the latter, which refers to a container typically used on supercomputing clusters, will be covered in a later tutorial. For now, we will set it to ``docker``. The second block of code reformats the ``mem`` variable to remove the suffix ``gb``, so that it can be read by fMRIPrep.

Next we come last half of the code: an ``if/else`` statement that executes code depending on whether you chose ``docker`` or ``singularity``. Since we chose ``docker``, the second part of the statement will be run. Within that section, we will supply both the root directory containing the data - in other words, the ``Flanker`` directory - and the data where the output will be stored, which we will place in the ``derivatives`` subfolder.

The other lines in this block mostly contain options for your analysis, which we will explore later. For now, we will run a relatively simple analysis which does the standard preprocessing steps of coregistration, normalization, and physiological component extraction. The last two lines, ``--mem_mb`` and ``-w``, use variables to specify the amount of memory to be used, and the working directory where intermediate results will be stored.


Running the Script
******************

To run the script, simply navigate to the ``code`` directory and type the following:

::

  bash fmriprep.sh
  
This will begin preprocessing the data for subject #8 - which, you may recall, was one of the first subjects we analyzed in the fMRI tutorials on SPM, AFNI, and FSL. Our goal here will be to compare the output from those processing pipelines with what is generated by fMRIPrep, in order to see the relative advantages and disadvantages of each.

Using the barebones analysis pipeline that we specified above, this should take about one or two hours to process. When it has finished, click the ``Next`` button.

Video
*****

For a video demonstration of how to set up the fmriprep.sh script, click `here <https://www.youtube.com/watch?v=qCX4YlrdTAw>`__.
