.. _01_Stats_TimeSeries.rst

The Time-Series
***********

Now that the first functional run has been preprocessed, we can **fit a model** to the data. To understanding how model fitting works, we need to review how fMRI data is composed. Remember that fMRI datasets contain several **volumes** strung together like beads on a string - we call this concatenated string of volumes a **time-series**.

To illustrate what this looks like, open up the fsleyes viewer and load the dataset ``filtered_func_data.nii.gz``. In the lower right corner is a window labeled "Location", with a field called ``Volume``. This indicates the current volume in the time-series that is displayed in the viewing window. Click up the up arrow next to the field to display the next volume in the time-series, noting how there are small but noticeable changes from one volume to the next.

.. note::
  To see the time-series update at a quicker, continuous pace, click on the Movie Reel icon. The update rate can be changed by clicking on the Wrench icon.

Then, click on View menu at the top of the screen and select ``Time series``. This opens up another window that displays changes in signal across the entire time-series, with the volume number on the x-axis. The y-axis is measured in arbitrary units of fMRI signal that are collected by the scanner; these units will be interpretable after we normalize them for each scan and compare this normalized signal between conditions.

.. figure:: TimeSeriesDemo.gif


Remember that our goal is to create a model that will fit the time-series at each voxel. To understand how this model is constructed, click the ``Next`` button.
