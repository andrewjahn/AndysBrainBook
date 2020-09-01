.. _ML_01_Brown_Example:

========================================================================
Machine Learning Tutorial #1: Basic Example with Support Vector Machines
========================================================================

-----------

Overview
********

This tutorial is transcribed almost verbatim from the `Brown University website <https://www.brown.edu/carney/mri/researchers/analysis-pipelines/mvpa>`__; the only additions here will be figures to show the results from certain steps, and to provide clarification where necessary.

The study design, according to the website, was as follows:

  We presented a participant with 4 different types of visual stimuli (cars, shoes, faces, houses) in a blocked design. The participant passively viewed these images (no task was performed). The study consisted of 8 runs, where each run was comprised of four blocks, one for each stimulus category. Within each block there were 10 images from a single simulus condition. The blocks were randomized across each run, and no image was repeated across runs (80 images per category total).
  
This dataset can be downloaded `here <https://drive.google.com/drive/folders/0B141z-GC_3Bdbms5TGlGRU9DRlk>`__. By highlighting all of the items and right-clicking, you can select "Download". This will place the files within your ``Downloads`` folder.
  
Pre-processing
**************

The data for this study has already been preprocessed, which includes sclie-timing correction, motion correction, and spatial smoothing. Note that some studies do not perform spatial smoothing, in order to keep the activation profiles of each voxel as separate as possible; because we are not concerned with detecting the strength of a signal that is there, we do not need to average together the signal of nearby voxels. We will cover this more in a later tutorial, in which you compare the results both with and without smoothing.

Regression Analysis
*******************

The .1D files that were included in the dataset indicate which stimulus class was presented during which run. Using ``3dDeconvolve``, we use the ``stim_times_IM`` option which will estimate the amplitude of the BOLD response for each trial within that stimulus class. Since there were 8 trials for each stimulus class, and 4 stimulus classes, we will have 32 regressors total: 

::

  3dDeconvolve -input run1.preproc.nii run2.preproc.nii run3.preproc.nii run4.preproc.nii run5.preproc.nii run6.preproc.nii run7.preproc.nii run8.preproc.nii \
  -polort 1 \
  -local_times \
  -censor allRuns.censor.1D \
  -num_stimts 4 \
  -stim_times_IM 1 cars.block.onsets.1D 'BLOCK(9,1)' -stim_label 1 cars \
  -stim_times_IM 2 faces.block.onsets.1D 'BLOCK(9,1)' -stim_label 2 faces \
  -stim_times_IM 3 houses.block.onsets.1D 'BLOCK(9,1)' -stim_label 3 houses \
  -stim_times_IM 4 shoes.block.onsets.1D 'BLOCK(9,1)' -stim_label 4 shoes \
  -bucket MVPA.BLOCK.nii
  
  
We will then extract the sub-briks from each class in the output of 3dDeconvolve, ``MVPA.BLOCK.nii`, using a for-loop. For example, the line of code:

::

  for a in $(seq 1 8); do 3dTcat -prefix cars.$a.nii MVPA.BLOCK.nii[${a}]; done
  
Will create 8 files, cars.1.nii, cars.2.nii, all the way until cars.8.nii. (The ``seq`` command creates a string of numbers between the two arguments that are provided; in this case, 1, 2, 3, 4, 5, 6, 7, and 8.)


::

  for a in $(seq 1 8); do 3dTcat -prefix cars.$a.nii MVPA.BLOCK.nii[${a}]; done
  for a in $(seq 9 16); do (( b=`expr $a - 8` )); 3dTcat -prefix faces.$b.nii MVPA.BLOCK.nii[${a}]; done
  for a in $(seq 17 24); do (( b=`expr $a - 16` )); 3dTcat -prefix houses.$b.nii MVPA.BLOCK.nii[${a}]; done
  for a in $(seq 25 32); do (( b=`expr $a - 24` )); 3dTcat -prefix shoes.$b.nii MVPA.BLOCK.nii[${a}]; done
