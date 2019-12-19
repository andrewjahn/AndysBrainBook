.. _SPM_01_Stats_TimeSeries:

Chapter 1: The Time-Series
***********

To understanding how model fitting works, first we need to review the structure of fMRI data. Remember that fMRI datasets contain several **volumes** strung together like beads on a string; we call this concatenated string of volumes a **run** of data. The signal that is measured at each voxel across the entire run is called a **time-series**. It follows that there are as many time-series as there are voxels in your dataset.

.. note::

  In SPM, a run is called a **session**. Some terms have not been standardized across the analysis packages, but for the purporses of this course I will continue with the above definition of a run.

In SPM, unfortunately, there is no easy way to view the time-series of a volume. (Expand on this later, maybe talk about a workaround with spm_vol.)


You now have a better understanding of how the time-series represents the signal that is measured at each voxel, but where does that signal come from? In the next chapter we will briefly review the history of fMRI and how we generate the signal you see in the viewer.
