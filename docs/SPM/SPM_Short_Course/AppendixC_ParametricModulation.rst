.. _AppendixC_ParametricModulation:

=================================
Appendix C: Parametric Modulation
=================================

------------------

Overview
********

A review of parametric modulation, as well as the dataset we will be analyzing, can be found :ref:`here <PM_Overview>`. This tutorial will focus on preprocessing the data and using the parametric modulators in the general linear model.

Preprocessing the Data
**********************

------------------

To prepare the data for a parametric modulation analysis, we will create a preprocessing pipeline similar to the one we used for the example dataset :ref:`here <fMRI_Intro>`. If you haven't already, go through the tutorial with the Flanker dataset; there you can find the details of each preprocessing and statistical analysis step, which we won't discuss here in depth.

Creating the Timing Files
^^^^^^^^^^^^^^^^^^^^^^^^^

We will first create timing files that contain onsets for the Gamble, the parametric value for the potential Gain, and the parametric value for the potential Loss, for each run; in total, we will create nine regressors.

You can download a script to convert the timings into a format that FSL understands by clicking `here <https://github.com/andrewjahn/FSL_Scripts/blob/master/make_FSL_Timings_Gambles.sh>`__, clicking on the ``Raw`` button, and right-clicking anywhere on the screen and selecting ``Save As``. Save the file as a shell script into the ``Gambles`` directory. Once it has been downloaded, navigate to that directory with a Terminal and run the script by typing ``bash make_FSL_Timings_Gambles.sh``. You should see nine text files in each ``func`` directory:

.. figure:: Appendix_F_Onsets.png

