.. _fMRIPrep_Demo_6_GroupAnalysis:

====================================
fMRIPrep Tutorial #6: Group Analysis
====================================

---------

Overview
********

Our group-level analysis requires that we preprocess all of the individual subjects and run 1st-level analyses for each of them. To do that, we will put our preprocessing and 1st-level analysis code in a for-loop.

Modifying the fmriprep.sh Script
********************************

Previously, we used the ``fmriprep.sh`` script that had a hard-coded subject number. To put this code into a for-loop, we will need to include this line of code just below the first line:

::

  subject=$1
  
And modify the "subj" line to the following:

::

  subj=$subject
  
Also make sure to download the script "doDecon.sh" to your Flanker directory.
  
Running the for-loop
********************

The code will look like this:

::

  for i in `cat subjList.txt`; do
    cd code;
    bash fmriprep.sh $i;
    cd ../derivatives/fmriprep/${i}/func
    for run in 1 2; do
      3dmerge -1blur_fwhm 4.0 -doall -prefix r${run}_blur.nii \
          ${i}_task-flanker_run-${run}_space-MNI152NLin2009cAsym_res-2_desc-preproc_bold.nii.gz;
      done
    for run in 1 2; do
      3dTstat -prefix rm.mean_r${run}.nii r${run}_blur.nii;
      3dcalc -a r${run}_blur.nii -b rm.mean_r${run}.nii \
         -c ${i}_task-flanker_run-${run}_space-MNI152NLin2009cAsym_res-2_desc-brain_mask.nii.gz                            \
         -expr 'c * min(200, a/b*100)*step(a)*step(b)'       \
         -prefix r${run}_scale.nii;
      done
      rm rm*;
    mkdir stimuli;
    cp ../../../../${i}/func/*.1D stimuli;
    cp ../../../../doDecon.sh .;
    cp sub-${i}_task-flanker_run-2_desc-confounds_regressors.tsv run-2_confounds_regressors_BACKUP.tsv
tail -n +2 sub-${i}_task-flanker_run-2_desc-confounds_regressors.tsv > tmp2.txt
    NT=`3dinfo -nt r1_scale.nii`
    for ((i=0; i<$NT; i++)); do echo 0 >> tmp.txt; done #CHANGE i VARIABLE
    cat tmp.txt tmp2.txt > sub-${i}_task-flanker_run-2_desc-confounds_regressors.tsv
    tcsh doDecon.sh $i;
    cd ../../../../code;
  done
    
