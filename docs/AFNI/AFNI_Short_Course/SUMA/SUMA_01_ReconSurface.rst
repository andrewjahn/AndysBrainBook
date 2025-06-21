.. _SUMA_01_ReconSurface:

=====================================================
SUMA Tutorial #1: Reconstructing the Cortical Surface
=====================================================

-------------

Introduction
************

The first step of a surface-based analysis is to reconstruct the cortical surface from a three-dimensional volume. This is discussed in detail in the :ref:`FreeSurfer Short Course <FS_01_BasicTerms>`. Here, we will run a script to process each of our subject's anatomical datasets using FreeSurfer's :ref:`recon-all command <FS_03_ReconAll>`.

Running recon-all
*****************

If you have FreeSurfer and the :ref:`parallel command <FS_04_ReconAllParallel>` installed, you will be able to loop the recon-all command over all of your subjects. First, navigate to the ``Flanker`` directory containing all of your subjects, and type the following:

::

  mkdir FS
  
We will be running all of our FreeSurfer analyses in this directory, and storing the output there as well. To do that, navigate into the FS directory (i.e., type ``cd FS``), and run the following code:

::

  #!/bin/bash
  
   # Check whether the file "subjList.txt" exists; if it doesn't, create a file containing all of the subject names in our study 
   
  if [ ! -f subjList.txt ]; then
     ls .. | grep ^sub- > subjList.txt
  fi
  
  # Copy each subject's anatomical file into the current directory, unzip the file, and set the current directory as FreeSurfer's SUBJECTS_DIR. Then process each of the anatomical files with recon-all using the "parallel" command

  for sub in `cat subjList.txt`; do
      cp ../${sub}/anat/*.gz .
  done

  gunzip *.gz

  SUBJECTS_DIR=`pwd`

  ls *.nii | parallel --jobs 8 recon-all -s {.} -i {} -all -qcache

  # Clean up

  rm *.nii
  
A copy of this file can be downloaded `here <https://github.com/andrewjahn/AFNI_Scripts/blob/master/SUMA/runFreeSurfer.sh>`__.

Once you have downloaded the script (or copied and saved the above text into a file called ``runFreeSurfer.sh``), you can run it by typing:

::

  bash runFreeSurfer.sh

Monitoring the Progress of Recon-all
************************************

The above script will take a few days to finish. As the script runs, it will generate directories with the names ``sub-01_T1w``, ``sub-02_T1w``, and so on, one for each subject in your study. You can check on the status by looking at the file ``recon-all.log`` in the ``scripts`` sub-directory, which is updated each time FreeSurfer completes a step of the preprocessing. The full path to the file for sub-01, for example, would be ``sub-01_T1w/scripts/recon-all.log``. Open up the file in a text editor, and scroll to the bottom. If everything has finished without errors, you should see something like the following:

::

  recon-all -s sub-01_T1w finished without error at Wed Sep 11 04:22:01 EDT 2019
  
If recon-all exited with errors, on the other hand, you will see text that says ``Exited with ERRORS``. Else, recon-all is probably still running, and you should check on it later.
