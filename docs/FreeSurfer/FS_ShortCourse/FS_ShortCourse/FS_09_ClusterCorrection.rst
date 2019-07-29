.. _FS_ShortCourse/FS_09_ClusterCorrection:

======================================
FreeSurfer Tutorial #9: Cluster Correction
======================================

---------------

Overview
*********

After you have run your general linear model and created group-level contrast maps, you will need to correct for the amount of tests that you have run.


Cluster Correction with mri_glmfit-sim
**********

As in the previous tutorial, we will use nested for-loops to create cluster-corrected maps for each contrast. Each level of the nested loop specifies a different combination of which hemisphere, smoothness, and structural measurement we are analyzing:

::

  #!/bin/tcsh
  
  setenv study $argv[1]
  
  foreach meas (thickness volume)
    foreach hemi (lh rh)
      foreach smoothness (10)
        foreach dir ({$hemi}.{$meas}.{$study}.{$smoothness}.glmdir)
          mri_glmfit-sim \
            --glmdir {$dir} \
            --cache 1.3 pos \
            --cwp 0.05  \
            --2spaces
        end
      end
    end
  end
  
  
The options for mri_glmfit_sim specify the following:

1. The directory that is being corrected for multiple comparisons (``--glmdir``);
2. The vertex-wise cluster threshold (``--cache``);
3. The cluster-wise p-threshold (``--cwp``, always set to 0.05 unless you have reasons for doing otherwise);
4. Correction for analyzing both hemispheres (``--2spaces``)

For most analyses, the ``--glmdir``, ``--cwp``, and ``--2spaces`` options will not need to be changed. The ``--cache`` option, on the other hand, may be changed depending on your analysis; and so it deserves to be explained in more detail.

The first argument to the ``--cache`` option is the **vertex-wise threshold** - only vertices above this threshold will be considered part of the clusters that will then be tested for statistical significance. The following table shows you which vertex-wise thresholds have already been **cached** by FreeSurfer, or already stored in memory. In other words, FreeSurfer has pre-computed the number of contiguous vertices at each vertex-wise threshold in order to be labeled a significant cluster:

==================   ============
-log10(P) value      p-value
==================   ============
1.3                  0.05
2.0                  0.01
2.3                  0.005
3.0                  0.001
3.3                  0.0005
4.0                  0.0001
==================   ============


-----------


Video
**********

For a video overview of how to do cluster correction in FreeSurfer, click `here <https://www.youtube.com/watch?v=CpnKJWdW1Pc&list=PLIQIswOrUH6_DWy5mJlSfj6AWY0y9iUce&index=9>`__.
