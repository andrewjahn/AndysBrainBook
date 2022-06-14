.. _CAT12_03_Preprocessing:

=========================================
CAT12 Tutorial #3: Preprocessing the Data
=========================================

Overview
********

Compared to analyzing fMRI data, structural data is relatively quick and easy: The defaults work fine for most purposes, and the main preprocessing step is segmentation. This takes about 30 minutes per subject depending on which options you use, but given that there is usually one anatomical image per subject, about half an hour is usually the upper limit. This can be longer for longitudinal studies which involve separate anatomical scans being collected at each timepoint, but for now we will focus on a cross-sectional comparsion between Alzheimer's patients and control subjects.

Segmenting the Data
*******************

If you don't already have CAT12 open from the previous chapter, open a Matlab terminal, and type ``spm fmri``. (SPM12 should already be installed; if it isn't, see the previous chapter about downloading and installing the CAT12 toolbox.) Select the CAT12 toolbox from the ``toolbox`` dropdown menu, and click on the ``Segment`` button in the CAT12 GUI. This will open a new batch editor window:

.. figure:: 03_CAT12_Segmentation_Window.png

As you recall from the SPM12 tutorials, an ``X`` indicates the fields that need to be filled in before the module can be run. Double-click on the ``Volumes`` field, and navigate to the ``CAT12_Tutorial`` directory on your Desktop. Go into the ``AD`` directory, and select each subject's anatomical image, going in the order that the subjects are listed in the GUI window; then, go into the ``CN`` directory and do the same. Alternatively, you can use the file selection window to navigate to the ``CAT12_Tutorial`` directory containing both the ``AD`` and ``CN`` directories, enter the string ``nii.*`` in the ``Filter`` field, and click the ``Rec`` button to recursively search all the sub-folders in the current directory for any files ending in ``nii``. You should see something like this:

.. figure:: 03_CAT12_Select_Files.png

We will leave the rest of the options as the defaults. For now, however, take a look at the field ``Split job into separate processes``. The default value is ``4``, which means that four processors, if available, will be used to analyze the data. Click the green Go button, and observe what happens:

.. figure:: 03_CAT12_Preprocessing.png

Note that there are four instances of Matlab now running which are analyzing the data: three subject have been assigned to each processor, and each subject currently in each processor's queue are being analyzed simultaneously. This is a preview of what we will be doing on a much larger scale when we analyze more subjects using the supercomputing cluster. Each processor's logfile is also open, click between the different logfiles to see how they update as the data is analyzed. This will take a couple of hours; we will come back to it when it has finished.
