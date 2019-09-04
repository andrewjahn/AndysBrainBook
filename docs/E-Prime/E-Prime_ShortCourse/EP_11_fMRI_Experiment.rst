.. _EP_11_fMRI_Experiment:

=============================================
E-Prime Tutorial #11: Making an fMRI Experiment
=============================================

.. note::

  This section is under construction.

Overview
***********************

Once you have created a behavioral experiment, you may think that all you need to do is present it to the subjects while they are in the MRI scanner. However, there are certain aspects of the experiment that we need to change, such as inserting **jitters** between the trials that we are interested in.

Another point: You should make sure that you are detecting a behavioral effect before you scan the subject. fMRI data is very noisy, and the odds are small that you will detect a neural effect without a corresponding behavioral effect.


The Trigger Pulse
************************

When a scanner begins to collect images, it sends a trigger pulse, more commonly known as a **trigger**, to the experimental computer. In order to ensure that all of our timings are aligned to a common reference point - in other words, that we have a point in our experiment representing the start of our timings at zero seconds - we will need to include an object whose purpose is to wait for the trigger, and then begin the experiment when the trigger is detected.


----------------

Video
***********

To watch a video about how to format your E-Prime experiment for an MRI scanner, click `here <https://www.youtube.com/watch?v=FeC0SLWC7B0&list=PLIQIswOrUH68zDYePgAy9_6pdErSbsegM&index=11>`__.
