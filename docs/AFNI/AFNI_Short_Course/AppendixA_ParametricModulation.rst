.. _AppendixA_ParametricModulation:

=========================================
Appendix A: Parametric Modulation in AFNI
=========================================

-----------------

Overview
********

This tutorial will show you how to set up a parametric modulation analysis in AFNI. A review of parametric modulation, as well as the dataset we will be analyzing, can be found :ref:`here <PM_Overview>`. This tutorial will focus on preprocessing the data and using the parametric modulators in the general linear model.

Preprocessing the Data
**********************

------------------

Creating the Timing Files
^^^^^^^^^^^^^^^^^^^^^^^^^

We will first create timing files that contain onsets for the Gamble, the parametric value for the potential Gain, and the parametric value for the potential Loss, for each run; in total, we will create nine regressors.

You can download a script to convert the timings into a format that SPM understands by clicking `here <https://github.com/andrewjahn/FSL_Scripts/blob/master/make_FSL_Timings_Gambles.sh>`__, clicking on the ``Raw`` button, and right-clicking anywhere on the screen and selecting ``Save As``. Save the file as a shell script into the ``Gambles`` directory. Once it has been downloaded, navigate to that directory with a Terminal and run the script by typing ``bash make_FSL_Timings_Gambles.sh``. You should see nine text files in each ``func`` directory:

.. figure:: Appendix_F_Onsets.png

Creating the Preprocessing Script
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Similar to the :ref:`scripting section <AFNI_06_Scripting>` of the SPM tutorial, we will create a Batch script that can be used in a for-loop to analyze all of our subjects. Assuming you are familiar already with the details of preprocessing, we won't go over how to fill in each of the fields.
