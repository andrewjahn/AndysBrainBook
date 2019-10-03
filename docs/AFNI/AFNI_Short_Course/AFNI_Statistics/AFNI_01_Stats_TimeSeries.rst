.. AFNI_01_Stats_TimeSeries:

AFNI Statistical Modeling, Chapter 1: The Time-Series
***********

To understanding how model fitting works, first we need to review the structure of fMRI data. Remember that fMRI datasets contain several **volumes** strung together like beads on a string; we call this concatenated string of volumes a **run** of data. The signal that is measured at each voxel across the entire run is called a **time-series**. It follows that there are as many time-series as there are voxels in your dataset.

.. note::

  In SPM, a run is called a **session**. Some terms have not been standardized across the analysis packages, but for the purporses of this course I will continue with the above definition of a run.

To illustrate what this looks like, navigate to the folder ``sub-01``, open up the AFNI viewer and load the dataset ``filtered_func_data.nii.gz``. In the lower right corner is a window labeled ``Location``, with a field called ``Volume``. This indicates the current volume in the time-series that is displayed in the viewing window. Click up the up arrow next to the field to display the next volume in the time-series, noting how there are small but noticeable changes from one volume to the next.

.. note::
  To see the time-series update at a quicker, continuous pace, click on the Movie Reel icon. The update rate can be changed by clicking on the Wrench icon.

Then, click on the ``View`` menu at the top of the screen and select ``Time series``. This opens up another window that displays changes in signal across the entire time-series, with the volume number on the x-axis. The y-axis is measured in arbitrary units of fMRI signal that are collected by the scanner; these units will be interpretable after we normalize them for each scan and compare this normalized signal between conditions.

.. figure:: TimeSeriesDemo.gif


The time-series represents the signal that is measured at each voxel, but where does that signal come from? In the next chapter we will briefly review the history of fMRI and how we generate the signal you see in the viewer.
