.. _fMRI_07_2ndLevelAnalysis:

fMRI Tutorial #7: 2nd-Level Analysis
=================

Overview
***********

Once you have preprocessed and analyzed all of the runs for all of the subjects in the Flanker dataset, you are ready to run a **2nd-level analysis**. Whereas AFNI and SPM define a 2nd-level analysis as synonymous with a group analysis, in FSL a 2nd-level analysis is the averaging together of the 1st-level analyses within each subject. In this dataset, that means calculating the average of the parameter estimates and contrast estimates for each subject.

From the Flanker directory, open the FEAT GUI from the command line by typing ``Feat_gui``. Then from the dropdown menu select ``Higher-Level Analysis``. This changes the input field to ``Select FEAT directories``.

.. note::

  The dropdown menu on the Data tab allows you to choose either ``Inputs are lower-level FEAT directories`` (the default), or ``Inputs are 3D cope images from FEAT directories``. Choosing the FEAT directories will allow you select which cope images to analyze, although directly selecting the cope images can give you more flexilibity if you didn't analyze the data with FSL's default processing stream (i.e., the data is not organized in a FEAT directory).
  
  
Since we had 26 subjects with 2 runs each, we have 56 FEAT directories total. Change the ``Number of inputs`` to 56, and then click the button ``Select FEAT directories``.

You *could* select each of the FEAT directories by hand, clicking on the Folder icon and selecting each one individually. But as we saw with scripting our analysis, this usually isn't a good idea - it is impractical for large datasets, and the odds of making a mistake increases with the number of subjects.

Instead, we will use `wildcards <https://andysbrainbook.readthedocs.io/en/latest/unix/Unix_07_Scripting.html#wildcards>`__ to make this quicker and easier. Go back to your Terminal that you launched the FEAT GUI from, and type ``ctrl+z``, and then type ``bg`` and press Enter. This will allow you to type commands in the Terminal while keeping the FEAT GUI open. At the command line, type the following:

::

  ls -d $PWD/sub-??/run*
  
This will print an absolute path to each FEAT directory. The ``-d`` option means to only list directories, and ``$PWD`` expands to an absolute path pointing to the current working directory. Within the current directory, any directory starting with ``sub-`` and ending with two digits (represented by the ``?`` wildcards) is added to the path. Finally, within each subjects directory, any directory beginning with the string ``run`` will be appended to the path name (e.g., run1.feat and run2.feat).

This will create a list with 52 entries, one corresponding to each run for each subject in the study. Highlight the entire list and copy it by pressing ``command+c``. This will copy the list to your clipboard. Then go back to the ``Select input data`` window, and click on the ``Paste`` button. Click in the ``Input data`` window, and then press ``ctrl+y`` and click ``OK``. This will paste the list of directories into the corresponding rows in the ``Select input data`` window.

.. figure:: 2ndLevelAnalysis_SelectingFEATDirectories.png

  A list of FEAT directories can be generated in the Terminal using a combination of variables and wildcards (A). Clicking on the ``Paste`` button in (C) will open up an ``Input data`` window, in which the list of directories can be pasted (B).
  
