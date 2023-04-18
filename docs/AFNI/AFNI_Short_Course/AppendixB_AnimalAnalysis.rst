.. _AppendixB_AnimalAnalysis:

==============================
Appendix B: Analyzing Rat Data
==============================


Overview
--------

In addition to analyzing human brains, AFNI can be used to analyze animal brains as well. In this tutorial, we will replicate the results of a recent *Nature* paper published by Sirmpilatze, Baudewig, & Boretius (2019); a link to the paper can be found `here <https://www.nature.com/articles/s41598-019-53144-y#data-availability>`__, and a link to the data on OpenNeuro can be found `here <https://openneuro.org/datasets/ds001981/versions/1.0.3>`__.

The aim of the study was to examine how fMRI measures from task-based and resting-state analysis change over time when rats are anesthetized with Medetomidine. For our purposes, we will simply replicate the task-based effects of electrical forepaw stimulation (or EFS), as well as the resting-state results. The advanced user may choose to replicate their temporal variability analyses, but that is beyond the scoope of the current tutorial.

Preprocessing the Data
**********************

The Methods section of the paper states that the data were analyzed using Python tools, which called upon a combination of FSL and AFNI commands; nevertheless, we will use mostly AFNI commands here, as well as an advanced registration tool called ANTs. In particular, we will replicate the following preprocessing steps were done:

1. Slice-timing correction;
2. Band-pass filtering of less than 0.01Hz and greater than 0.15Hz;
3. Spatial smoothing of 0.5mm.
4. Coregistration between the functional and structural images;

Note that in this study motion correction was not performed, since the rats were anesthetized and did not move much. Consequently, motion regressors were not used in the model.

Let's begin with slice-timing correction. Using ``3dinfo`` on the first EFS run, we can see the following the in the header:

::

  Dataset File:    sub-01_task-efs_run-01_bold.nii.gz
  Identifier Code: NII_GMnEUUfC2G6z4SOYguib2A  Creation Date: Tue Apr 18 12:27:02 2023
  Template Space:  ORIG
  Dataset Type:    Echo Planar (-epan)
  Byte Order:      LSB_FIRST {assumed} [this CPU native = LSB_FIRST]
  Storage Mode:    NIFTI
  Storage Space:   324,403,200 (324 million) bytes
  Geometry String: "MATRIX(0.2,0,0,-12.8,0,-0.2,0,9.4,0,0,0.5,-5.94792):128,96,30"
  Data Axes Tilt:  Plumb
  Data Axes Orientation:
    first  (x) = Right-to-Left
    second (y) = Posterior-to-Anterior
    third  (z) = Inferior-to-Superior   [-orient RPI]
  R-to-L extent:   -12.800 [R] -to-    12.600 [L] -step-     0.200 mm [128 voxels]
  A-to-P extent:    -9.600 [A] -to-     9.400 [P] -step-     0.200 mm [ 96 voxels]
  I-to-S extent:    -5.948 [I] -to-     8.552 [S] -step-     0.500 mm [ 30 voxels]
  Number of time steps = 220  Time step = 1.50000s  Origin = 0.00000s
    -- At sub-brick #0 '?' datum type is float
    -- At sub-brick #1 '?' datum type is float
    -- At sub-brick #2 '?' datum type is float
  ** For info on all 220 sub-bricks, use '3dinfo -verb' **
  
The TR is 1.5s, there are 220 volumes in this run, and the voxel resolution is 0.2x0.2x0.5mm, which explains why the resolution in the sagittal and coronal planes looks lower than in the axial plane.

To slice-time correct this data, type:

::

      3dTshift -tzero 0 -quintic -prefix sub-01_run-01_STC.nii sub-01_task-efs_run-01_bold.nii.gz

Next, we bandpass the data using 3dBandPass:

::

  3dBandpass -prefix sub-01_run-01_STC_BP.nii 0.01 0.15 sub-01_run-01_STC.nii
  
And then smooth the data with a 0.5mm kernel:

::

  3dmerge -1blur_fwhm 0.5 -doall -prefix sub-01_run-01_STC_BP_Smoothed.nii sub-01_run-01_STC_BP.nii

First-Level Analysis
********************

Looking within the task-efs_events.tsv file, we find that the onsets were:

::

  onset   duration
  60      30
  150     30
  240     30
  
This is the same for all of the subjects in the study, except for subject sub-07 (see note on the OpenNeuro repository). 
