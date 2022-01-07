.. _FIR_01_AFNI:

================================
Chapter #1: FIR Analysis in AFNI
================================

------------------

Overview
********

In AFNI, the individual time-points you estimate are called TENTs - possibly because each basis function looks like a pup tent. The peak of this TENT function is estimated for each time-point that is specified, including the end-points of the interval; for example, if you wanted to estimate each time-point from zero (that is, when the stimulus occurs) to twelve seconds after the start of the stimulus, there would be seven estimates total: at the time-points 0, 2, 4, 6, 8, 10, and 12.

For this analysis, we will make edits to the ``cmd`` file provided on the study's associated `NITRC website <https://www.nitrc.org/projects/frcl>`__ (the directory can be found by clicking on the button ``See All Files`` next to the Download dropdown menu, and selecting ``afni_hrf_tent.tar.gz``). This file uses the command ``afni_proc.py`` to create a script with all of the individual AFNI commands needed to do each step of the preprocessing and statistical analysis.

Editing the Script
******************

To use the script in a for-loop to analyze all of the subjects in this dataset, we will change the ``set subj`` line to equal ``$1``, so that our input from the for-loop will be the subject identification code. To save time, we will also insert the line ``-regress_run_clustsim no``. The edited script will look like this:

::

  #!/usr/bin/env tcsh

  # created by uber_subject.py: version 0.37 (April 14, 2015)
  # creation date: Tue Mar 22 11:56:09 2016

  # set data directories
  set top_dir = $PWD

  # set subject and group identifiers
  set subj      = $1
  set group_id  = ToneCounting

  # run afni_proc.py to create a single subject processing script
  afni_proc.py -subj_id $subj                                      \
          -script proc.$subj -scr_overwrite                        \
          -blocks tshift align tlrc volreg blur mask scale regress \
          -copy_anat $top_dir/anat/${subj}_T1w.nii.gz                    \
          -tcat_remove_first_trs 0                                 \
          -dsets $top_dir/func/${subj}_task-tonecounting_bold.nii.gz     \
          -volreg_align_to third                                   \
          -volreg_align_e2a                                        \
          -volreg_tlrc_warp                                        \
          -blur_size 4.0                                           \
          -regress_stim_times                                      \
              $top_dir/func/tone_counting_onset_times.txt               \
              $top_dir/func/tone_counting_probe_onsets.txt              \
          -regress_stim_labels                                     \
              tone_counting probe                                  \
          -regress_basis_multi                                     \
              'TENT(0,12,7)' 'TENT(0,14,8)'                        \
          -regress_censor_motion 0.3                               \
          -regress_make_ideal_sum sum_ideal.1D                     \
          -regress_run_clustsim no                                \
          -html_review_style none

  tcsh -xef proc.$subj |& tee output.proc.$subj
  
Either make these edits to the original script, or copy and paste the above code into a file called ``cmd.ap.AFNI_Tent``. The main difference between this analysis and previous ones using the canonical hemodynamic response function is the line ``-regress_basis_multi 'TENT(0,12,7)' 'TENT(0,14,8)'``, which uses a TENT analysis for the onset_times condition spanning an interval from 0 to 12 seconds with 7 TENTs each, and 8 TENTs spanning the interval 0 to 14 seconds for the probe_onsets condition. Make sure the above script is in the folder that contains all of the subjects, and then copy and paste the following code into your terminal:

::

  #!/bin/bash

  if [[ ! -e subjList.txt ]]; then
    ls | grep ^sub- > subjList.txt
  fi

  for i in `cat subjList.txt`; do 
    cp cmd.ap.AFNI_Tent $i; 
    cd $i; 
    tcsh cmd.ap.AFNI_Tent $i; 
    cd ..; 
  done
  
This will create the file ``subjList.txt`` (if it doesn't exist already), and use a for-loop to run the above ``cmd`` script for each subject. This will take a couple of hours to run.

Examining the Results
*********************

Once the preprocessing and analysis is finished, navigate to the directory ``sub-01/sub-01.results``. Similar to the results folder from the AFNI tutorials, we see output from all of the steps; however, if we look at the output from the statistical analysis by typing ``3dinfo -verb stats.sub-01+tlrc``, we see the following (truncated to just the tone_counting condition):

::

  -- At sub-brick #1 'tone_counting#0_Coef' datum type is float:       -50.34 to         60.68
  -- At sub-brick #2 'tone_counting#0_Tstat' datum type is float:     -6.03381 to       4.62057
     statcode = fitt;  statpar = 68
  -- At sub-brick #3 'tone_counting#1_Coef' datum type is float:     -64.2838 to       89.6593
  -- At sub-brick #4 'tone_counting#1_Tstat' datum type is float:     -5.57096 to       4.79652
     statcode = fitt;  statpar = 68
  -- At sub-brick #5 'tone_counting#2_Coef' datum type is float:       -119.9 to        102.47
  -- At sub-brick #6 'tone_counting#2_Tstat' datum type is float:     -5.04005 to       4.87468
     statcode = fitt;  statpar = 68
  -- At sub-brick #7 'tone_counting#3_Coef' datum type is float:     -132.215 to       136.358
  -- At sub-brick #8 'tone_counting#3_Tstat' datum type is float:     -5.70406 to       5.24604
     statcode = fitt;  statpar = 68
  -- At sub-brick #9 'tone_counting#4_Coef' datum type is float:     -100.065 to       99.7619
  -- At sub-brick #10 'tone_counting#4_Tstat' datum type is float:     -5.50184 to       5.14456
     statcode = fitt;  statpar = 68
  -- At sub-brick #11 'tone_counting#5_Coef' datum type is float:      -134.37 to       156.491
  -- At sub-brick #12 'tone_counting#5_Tstat' datum type is float:     -5.12016 to        6.7376
     statcode = fitt;  statpar = 68
  -- At sub-brick #13 'tone_counting#6_Coef' datum type is float:     -76.8142 to        77.716
  -- At sub-brick #14 'tone_counting#6_Tstat' datum type is float:     -4.45098 to        4.4271
     statcode = fitt;  statpar = 68
     
There are 14 sub-briks total for the tone_counting condition, with 7 beta weights (i.e., those labeled "Coef") and 7 T-statistics. In other words, we have an average beta estimate for each time-point in a 12-second window after the onset of each tone_counting condition; these beta weights can then be submitted to a group-level analysis just like the beta weights from a canonical HRF analysis.

