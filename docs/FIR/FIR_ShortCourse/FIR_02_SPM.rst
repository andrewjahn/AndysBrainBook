.. _FIR_02_SPM:

===============================
Chapter #1: FIR Analysis in SPM
===============================

------------------

Overview
********

Compared to FIR analysis AFNI, the implementation in SPM is quite easy. The preprocessing is the same as in the previous SPM tutorial on analyzing event-related data from the Flanker task; the only difference is in how the first-level model is specified for each subject.

Setting Up the First-Level Model
********************************

Once you have finished preprocessing, go to the SPM GUI and click the button ``Specify 1st-level``. Use the Terminal to create a new directory ``1stLevel`` within the directory ``sub-01``, and select that in the ``Directory`` field for the output. Set the ``Units for design`` to ``Seconds``, the ``Interscan interval`` to ``2``, and leave the rest of the defaults as they are. 

Add a new Subect/Session from the ``Data & Design`` field, and use the file navigator to select all of the functional data that begin with the prefix ``swr`` (i.e., those files that have been smoothed, warped, and realigned). Use a frame selector range of 1:104 to select all of the volumes within the dataset.

Create two Conditions, ``ToneCounting`` and ``ToneProbe``. 
