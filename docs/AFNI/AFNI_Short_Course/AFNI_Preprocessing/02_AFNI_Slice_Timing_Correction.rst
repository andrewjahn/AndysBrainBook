.. _02_AFNI_Slice_Timing_Correction:

==================================
Chapter 2: Slice-Timing Correction
==================================

-------------

Unlike a photograph, in which the entire picture is taken in a single moment, an fMRI volume is acquired in **slices**. Each of these slices takes time to acquire - from tens to hundreds of milliseconds. 

The two most commonly used methods for creating volumes are sequential and interleaved slice acquisition. Sequential slice acquisition acquires each adjacent slice consecutively, either bottom-to-top or top-to-bottom. Interleaved slice acquisition acquires every other slice, and then fills in the gaps on the second pass. Both of these methods are illustrated in the video below.

.. figure:: 04_02_SliceTimingCorrection_Demo.gif

As you'll see later on, when we model the data at each voxel we assume that all of the slices were acquired simultaneously. To make this assumption valid, the :ref:`time-series <Time_Series>` for each slice needs to be shifted back in time by the duration it took to acquire that slice. `Sladky et al. (2011) <https://www.sciencedirect.com/science/article/pii/S1053811911007245>`__ also demonstrated that slice-timing correction can lead to significant increases in statistical power for studies with longer TRs (e.g., 2s or longer), and especially in the dorsal regions of the brain.



Although slice-timing correction seems reasonable, there are some objections:

1. In general, it is best to not interpolate (i.e., edit) the data unless you need to;

2. For short TRs (e.g., around 1 second or less), slice-timing correction doesn't appear to lead to any significant gains in statistical power; and

3. Many of the problems addressed by slice-timing correction can be resolved by using a **temporal derivative** in the statistical model (discussed later in the chapter on model fitting).


For now, we will do slice-timing correction, using the first slice as the reference. (This is specified by the ``-tzero 0`` option of the 3dTshift command.)
