.. _SUMA_03_AnalysisOnTheSurface:

=========================================
SUMA Tutorial #3: Analysis on the Surface
=========================================

-----------

Overview
********

Surface-based analysis has many advantages, which we outlined earlier.

Once you have prepared the surfaces with @SUMA_Make_Spec_FS and @SUMA_AlignToExperiment, you can use the output with ``afni_proc.py``. We will create a template in ``uber_subject.py``, and then make the following changes:

1. Insert a ``surf`` block after the ``volreg`` block;
2. Remove the ``mask`` block;
3. Remove any references to blur estimation in the ``regress`` block (i.e., regress_est_blur_epits and regress_est_blur_errts);
4. Add the option ``-surf_anat`` which points to the surface anatomical image in the SUMA directory;
5. Add the option ``-surf_spec`` which points to the spec files in the SUMA directory. If you want to analyze the individual subject's surface, use the <subjID>_?h.spec files in the SUMA directories created by @SUMA_Make_Spec_FS. If you want to do a group analysis in standardized space, use the std.141.<subjID>_?h.spec files.

To create the preprocessing script, a typical ``afni_proc.py`` script would look like the following:

::

  #!/usr/bin/env tcsh

  # set subject and group identifiers
  set subj  = sub-08
  set gname = Flanker

  # set data directories
  set top_dir = /Users/ajahn/Desktop/${gname}/sub-08
  set anat_dir  = $top_dir/anat
  set epi_dir   = $top_dir/func
  set stim_dir  = $top_dir/func

  # run afni_proc.py to create a single subject processing script
  afni_proc.py -subj_id $subj                                            \
  -script proc.$subj -scr_overwrite                              \
  -blocks tshift align tlrc volreg surf blur scale regress       \
  -copy_anat $anat_dir/sub-08_T1w.nii.gz                         \
  -dsets                                                         \
  $epi_dir/sub-08_task-flanker_run-1_bold.nii.gz             \
  $epi_dir/sub-08_task-flanker_run-2_bold.nii.gz             \
  -tcat_remove_first_trs 0                                       \
  -align_opts_aea -giant_move                                    \
  -tlrc_base MNI_avg152T1+tlrc                                   \
  -volreg_align_to MIN_OUTLIER                                   \
  -volreg_align_e2a                                              \
  -volreg_tlrc_warp                                              \
  -surf_anat ~/Desktop/Flanker/FS/sub-08_T1w/surf/SUMA/sub-08_SurfVol+orig     \
  -surf_spec ~/Desktop/Flanker/FS/sub-08_T1w/surf/SUMA/std.141.sub-08_?h.spec  \
  -blur_size 4.0                                                 \
  -regress_stim_times                                            \
  $stim_dir/congruent.1D                                     \
  $stim_dir/incongruent.1D                                   \
  -regress_stim_labels congruent incongruent                              \
  -regress_basis 'GAM'                                         \
  -regress_censor_motion 0.3                                     \
  -regress_motion_per_run                                        \
  -regress_opts_3dD                                              \
  -jobs 8                                                    \
  -gltsym 'SYM: congruent -incongruent' -glt_label 1 Con-Inc \
  -gltsym 'SYM: incongruent -congruent' -glt_label 2 Inc-Con \
  -regress_reml_exec                                             \
  -regress_make_ideal_sum sum_ideal.1D                           


This script would work for analyzing ``sub-08``. To make it generalizable, we will make the same edits to the path as we did during the :ref:`volumetric analysis <AFNI_06_Scripting>`.

We will also cut this code (which should be found around lines 218-219):

:: 

  # set directory variables
  set surface_dir = ${PWD}/FS/${subj}_T1w/surf/SUMA
  
And paste it around line 35, before we ``cd`` into the output directory.

To download the script that already has these edits, click `here <https://github.com/andrewjahn/AFNI_Scripts/blob/master/SUMA/SUMA_proc_MNI.sh>`__. You can then loop it over all of your subjects with the following code:

::

  for i in `cat subjList.txt`; do
    tcsh SUMA_proc_MNI.sh $i;
    mv ${i}.results_SUMA $i;
  done

Downloading the Surface Template
********************************

Just as with our volumetric analyses, we will need to render our results on a surface template. Navigate to the directory containing your subjects, and type the following:

::

  afni_open -aw suma_MNI_N27.tgz
  tar xvzf suma_MNI_N27.tgz
  
This will download and unpack the MNI_N27 surface template, which we can then view our results on.

Loading the First-Level Results
*******************************

After the preprocessing script has finished, you will see a list of files output that have names similar to the ones you saw during the volumetric analysis. In this case, however, some of the files end in suffixes such as ``niml.dset``. These are datasets that can be loaded into SUMA using the ``-input`` option.

To load the statistics for the left hemisphere for sub-01, for example, navigate to the directory ``sub-01/sub-01.results_SUMA`` and type the following:

::

  suma -spec ../../suma_MNI_N27/std.141.MNI_N27_lh.spec -sv sub-01_SurfVol_Alnd_Exp+tlrc -input stats.sub-01.lh.niml.dset
  
You can then select the statistic sub-brik you would like to display, and change the p-value threshold accordingly.

.. note::

  If the results have been normalized to MNI space, then use the code specified above. If you have processed the data in the subject's native space and have not normalized the data, then you could use the subject's spec file located in his corresponding surf directory, and change the surface volue to <subjName>_SurfVol_Alnd_Exp+orig.
