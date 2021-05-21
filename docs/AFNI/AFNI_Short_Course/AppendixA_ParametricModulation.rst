.. _AppendixA_ParametricModulation:

=========================================
Appendix A: Parametric Modulation in AFNI
=========================================

-----------------

Overview
********

This tutorial will show you how to set up a parametric modulation analysis in AFNI. A review of parametric modulation, as well as the dataset we will be analyzing, can be found :ref:`here <PM_Overview>`. This tutorial will focus on preprocessing the data and using the parametric modulators in the general linear model.

Preprocessing the Data
**********************

------------------

Creating the Timing Files
^^^^^^^^^^^^^^^^^^^^^^^^^

We will first create timing files that contain onsets for the Gamble, the parametric value for the potential Gain, and the parametric value for the potential Loss, for each run; in total, we will create nine regressors.

You can download a script to convert the timings into a format that SPM understands by clicking `here <https://github.com/andrewjahn/AFNI_Scripts/blob/master/make_Gamble_Timings.sh>`__, clicking on the ``Raw`` button, and right-clicking anywhere on the screen and selecting ``Save As``. Save the file as a shell script into the ``Gambles`` directory. Once it has been downloaded, navigate to that directory with a Terminal and run the script by typing ``bash make_Gamble_Timings.sh``. Within each func directory you should now see a file called ``gamble_Timings.1D``, which looks like the following:

.. figure:: AppendixA_OnsetTimes.png

The first number is the onset of the gamble; the asterisk indicates that the numbers coming after it are the modulators, separated by a comma. We will use this in the model for each subject.

Creating the Preprocessing Script
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Similar to the :ref:`scripting section <AFNI_06_Scripting>` of the AFNI tutorial, we will create a Batch script that can be used in a for-loop to analyze all of our subjects. Instead of using the ``uber_subject.py`` GUI, however, we will use the command ``afni_proc.py``. The script can be downloaded `here <https://github.com/andrewjahn/AFNI_Scripts/blob/master/runAFNIproc.sh>`__; click on ``Raw``, right-click and choose ``Save As``, and download it to your ``Gambles`` directory. If we open the file, this is what is inside:

::

  #!/bin/tcsh
  set subj = $argv[1]

   afni_proc.py -subj_id ${subj} -script proc.${subj} -scr_overwrite -blocks tshift                                                  \
     align tlrc volreg blur mask scale regress -copy_anat                                                                  \
     $PWD/{$subj}/anat/{$subj}_T1w.nii.gz                     \
     -dsets                                                                                                                \
     $PWD/{$subj}/func/{$subj}_task-mixedgamblestask_run-01_bold.nii.gz \
     $PWD/{$subj}/func/{$subj}_task-mixedgamblestask_run-02_bold.nii.gz \
     $PWD/{$subj}/func/{$subj}_task-mixedgamblestask_run-03_bold.nii.gz \
     -tcat_remove_first_trs 0 -align_opts_aea -giant_move -tlrc_base                                                       \
     MNI_avg152T1+tlrc -volreg_align_to MIN_OUTLIER -volreg_align_e2a                                                      \
     -volreg_tlrc_warp -blur_size 4.0 -regress_stim_times                                                                  \
     $PWD/{$subj}/func/gamble_Timings.1D                          \
     -regress_stim_labels Gamble -regress_stim_types AM2 -regress_basis 'BLOCK(3,1)'     \
     -regress_censor_motion 0.3 -regress_motion_per_run -regress_opts_3dD                                                  \
     -jobs 8 -regress_make_ideal_sum sum_ideal.1D                                                  \
     -regress_run_clustsim no

  tcsh proc.${subj}

The first line uses the first argument, or input, as the subject number for the rest of the script. The blocks tshift, align, tlrc, and so on, indicate the standard preprocessing steps of slice-timing correction, realignment, normalization, etc. The major difference from analyzing a non-parametric modulation dataset is using ``AM2``, which stands for Ampltiude Modulation. This will orthogonalize the modulators with respect to the main regressor, but not with respect to each other; for more details on why this is important, see the link to the Mumford et al. paper down below.

Running the Analysis
********************

Now that we have a template preprocessing script, we can use it in a for-loop to analyze all of the subjects. Type the following:

::

  for i in `cat subjList.txt`; do
    tcsh runAFNIProc.sh $i;
  done
  
This will take a couple of hours. We will return when it has finished preprocessing.


Group-Level Analysis
********************

If you navigate into one of the output directories, such as ``sub-01.results``, and open AFNI, load the warped anatomical image as an underlay and the ``stats.sub-01.+tlrc`` image as an overlay:

::

  afni anat_final.sub-01+tlrc. stats.sub-01+tlrc.
  
In the ``Define Overlay`` window, there are multiple options in the ``OLay`` and ``Thr`` dropdown menus. The first one (``Gamble#0_Coef``) is the beta weight for the main regressor of "Gamble", while ``Gamble#1_Coef`` corresponds to the parametric modulator of Gain and ``Gamble#2_Coef`` corresponds to the parametric modulator of Loss. We will use these last two regressors as inputs into our group-level analysis.



.. figure:: AppendixA_GambleOverlays.png
