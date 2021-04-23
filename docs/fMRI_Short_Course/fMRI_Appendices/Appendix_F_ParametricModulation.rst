.. _Appendix_F_ParametricModulation:

=================================
Appendix F: Parametric Modulation
=================================

------------

Overview
********

This appendix demonstrates how to set up a **parametric modulation** analysis in FSL. An overview of what parametric modulation is can be found :ref:`here <PM_Overview>`, along with code for downloading the data. If you already have `Amazon Web Services <https://aws.amazon.com/cli/>`__ installed, then you can open a Terminal and type the following:

::

  aws s3 sync --no-sign-request s3://openneuro.org/ds000005 ds000005-download/
  mv ~/Downloads/ds000005-download ~/Desktop/Gambles
  
This will create a new folder on your Desktop called ``Gambles``, which we will use to store the data and the output from our analyses.

Preprocessing the Data
**********************

------------------

To prepare the data for a parametric modulation analysis, we will create a preprocessing pipeline similar to the one we used for the example dataset :ref:`here <fMRI_Intro>`. If you haven't already, go through the tutorial with the Flanker dataset; there you can find the details of each preprocessing and statistical analysis step, which we won't discuss here in depth.

Creating the Timing Files
^^^^^^^^^^^^^^^^^^^^^^^^^

 We will first create timing files that contain onsets for the Gamble, the parametric value for the potential Gain, and the parametric value for the potential Loss, for each run; in total, we will create nine regressors.

You can download a script to convert the timings into a format that FSL understands by clicking `here <https://github.com/andrewjahn/FSL_Scripts/blob/master/make_FSL_Timings_Gambles.sh>`__, clicking on the ``Raw`` button, and right-clicking anywhere on the screen and selecting ``Save As``. Save the file as a shell script into the ``Gambles`` directory. Once it has been downloaded, navigate to that directory with a Terminal and run the script by typing ``bash make_FSL_Timings_Gambles.sh``. You should see nine text files in each ``func`` directory:

.. figure:: Appendix_F_Onsets.png

Skull-Stripping the Brain
^^^^^^^^^^^^^^^^^^^^^^^^^

Before we open the FEAT GUI, we need to skull-strip the brain using ``bet2``. From the Gambles directory, navigate to the ``sub-01`` directory and type the following:

::

  bet2 anat/sub-01_T1w.nii.gz anat/sub-01_T1w_brain.nii.gz -f 0.2


Setting up the FEAT GUI
^^^^^^^^^^^^^^^^^^^^^^^

Now open the FEAT GUI by typing ``Feat_gui`` from the Terminal. Just as in the FSL tutorial, we will create a template design file that we can use in a for-loop to analyze all of the subjects.

In the ``Data`` tab, click on ``Select 4D data`` and navigate to the ``func`` folder; select the file ``sub-01_task-mixedgamblestask_run-01_bold.nii.gz`` and click OK. In the ``Output directory`` field, type ``run1``.

Leave the defaults as they are in the ``Pre-stats`` tab. In the ``Registration`` tab, select the file in the anat directory, ``sub-01_T1w_brain.nii.gz``. The search space is up to you, but to speed up the analysis, change ``Normal Search`` to ``Full Search`` for both the main structural image and the standard space image, and select ``12 DOF`` for both of them as well.

In the ``Stats`` tab, click on ``Full Model Setup``. Type the number ``3`` in the field ``Number of original EVs``, and label them in the following order:

1. Gambles
2. Gain_PM
3. Loss_PM

For each one, select ``Custom (3 column format)`` for the Basic Shape, and select the corresponding filename; e.g.,

1. Gambles -> gambles_run1.txt
2. Gain_PM -> gambles_gain_run1.txt
3. Loss_PM -> gambles_loss_run1.txt

In the ``Contrasts & F-tests`` tab, create ``4`` contrasts, and label them as follows with the corresponding contrast weights:

1. Gambles [1 0 0]
2. Gain_PM [0 1 0]
3. Loss_PM [0 0 1]
4. Gain-Loss_PM [0 1 -1]

When you are done, click on the ``Save`` button at the bottom of the GUI, and call it ``design_run1.fsf``. Save it in the ``Gambles`` directory. Do the same procedure for the other two runs, updating the functional run and the timing files, and saving the design files as ``design_run2.fsf`` and ``design_run3.fsf``. Save these files into the ``Gambles`` directory as well.

Next, download the file `run_1stLevel_Analysis_Gambles.sh <https://github.com/andrewjahn/FSL_Scripts/blob/master/run_1stLevel_Analysis_Gambles.sh>`__, saving it into the ``Gambles`` directory just like you did with the timing conversion script. Run it by typing:

::

  bash run_1stLevel_Analysis_Gambles.sh
  
You should see an HTML file open for each run that is analyzed, which will generate 48 tabs total. The entire analysis should take a four or five hours, depending on the speed of your machine.

Setting up the Second-Level Analysis
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^




