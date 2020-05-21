.. _AFNI_01_Stats_TimeSeries:

==========================
Chapter 1: The Time-Series
==========================

---------

To understanding how model fitting works, first we need to review the structure of fMRI data. Remember that fMRI datasets contain several **volumes** strung together like beads on a string; we call this concatenated string of volumes a **run** of data. The signal that is measured at each voxel across the entire run is called a **time-series**. It follows that there are as many time-series as there are voxels in your dataset.

.. note::

  In SPM, a run is called a **session**. Some terms have not been standardized across the analysis packages, but for the purporses of this course I will continue with the above definition of a run.

To illustrate what this looks like, navigate to the folder ``sub-08/func``, open up the AFNI viewer and load the dataset ``sub-08_task-flanker_run-1_bold.nii.gz``. Click on any one of the "Graph" buttons next to the viewing planes (Axial, Sagittal, or Coronal). At the bottom of the screen in red ink is text that says something like "indx=0 val=576 @t=0". This indicates that in the viewing planes you are currently seeing the first volume of the time-series (remember that in AFNI, indices start at 0), the value of the voxel at the center of your crosshairs is 576 arbitrary units of signal intensity, and that this particular volume was acquired at 0 seconds into the run (i.e., it was the first volume collected).

Click anywhere in the time-series window to select a particular volume, and then press up the left and right arrows to display the next volume in the time-series. Note how there are small but noticeable changes from one volume to the next. You may also notice that there are nine time-series being displayed simultaneously: The voxel at the center of your cross-hairs, and each adjacent voxel. You can change this montage by pressing ``m`` to remove one level of adjacent voxels, or ``M`` to see more voxels around the center voxels. You can also scale up or scale down the overall signal intensity of the time-series with ``+`` or ``-``. 

Play around with different combinations of these keystrokes to get a sense of the time-series, and also view it at different locations within and outside of the brain. What does the signal look in the grey matter, the white matter, and the ventricles? Do you notice any patterns? Is it possible that these patterns are related to the task that the subject is doing? We will answer this last question in the following chapters.

.. note::
  To see the time-series update at a quicker, continuous pace, click anywhere in the time-series window and press ``v``. You can stop the time-series video by pressing the spacebar. To change the rate of the video, from the AFNI GUI click "EditEnv", scroll to "AFNI_VIDEO_DELAY", and check the box next to it. In the field to the right, you can enter a value that specifies, in milliseconds, how long it will take to load the next volume in the time-series for the video.

.. figure:: 05_01_TimeSeriesDemo.gif


You now have a better understanding of how the time-series represents the signal that is measured at each voxel, but where does that signal come from? In the next chapter we will briefly review the history of fMRI and how we generate the signal you see in the viewer.
