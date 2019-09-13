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
5. Add the option ``-surf_spec`` which points to the spec files in the SUMA directory.


Loading the First-Level Results
********************************

After the preprocessing script has finished, you will see a list of files output that have names similar to the ones you saw during the volumetric analysis. In this case, however, some of the files end in suffixes such as ``niml.dset``. These are datasets that can be loaded into SUMA using the ``-input`` option.

to load the statistics for the left hemisphere, for example, type the following:

::

  suma -spec ${FS_DIR}/sub-01_T1w/surf/SUMA/test_lh.spec -sv sub01_SurfVol_Alnd_Exp+tlrc.HEAD -input stats.sub01.lh.niml.dset
  
You can then select the statistic sub-brik you would like to display, and change the p-value threshold accordingly.
