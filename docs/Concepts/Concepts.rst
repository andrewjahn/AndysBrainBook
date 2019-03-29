.. _Concepts::

==========
Overview
==========

This is a sketchpad for concepts and other things I've found useful; it will be organized later.


Resting-state smoothness estimate with AFNI's 3dFWHMx
------------

The AFNI team recommends using 3dFWHMx on the preprocessed data just before the regression step (e.g., after it has been fully preprocessed and smoothed). The commands I used were:

::

  3dAutomask -prefix mask+tlrc input.nii

  3dFWHMx -mask mask+tlrc -input input.nii -detrend -acf

  3dClustSim -acf 0.478139 6.31969 16.0042 -mask mask+tlrc -athr 0.05 -pthr 0.005
  3dClustSim -acf 0.478139 6.31969 16.0042 -mask mask+tlrc -athr 0.05 -pthr 0.001
  
  
To determine the cluster sizes at cluster-defining threshold of p=0.005 and p=0.001.


Why are the thresholds so much higher in resting-state than task-based data?


AFNI message board link is `here <https://afni.nimh.nih.gov/afni/community/board/read.php?1,152039,152969#msg-152969>`__.

