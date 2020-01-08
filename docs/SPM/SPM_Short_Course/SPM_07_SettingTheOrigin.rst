.. _SPM_07_SettingTheOrigin:

===========================
SPM Tutorial #7: Setting the Origin
===========================

----------

Overview
********

After we :ref:`scripted our analysis <SPM_06_Scripting>`, it would appear that all we need to do is run the script for the rest of the subjects. Before going on, it is wise to check whether the scripted analysis did as well as the analysis we did by hand for sub-08. If there are any errors in the script, they will propagate throughout all of our subjects - a good habit is to run the script for one or two subjects, and see whether there are any problems.

If you examine the output from each preprocessing step by loading the images into the Display viewer of the SPM GUI, you will notice that the inital preprocessing steps, such as realignment and slice-timing correction, appear to have run without any errors. When we look at the output of coregistration, however, we begin to see an anomaly: Although the functional data looks normal, the realigned anatomical image looks as though the orientations have been flipped. When we look at the output of later stages, such as normalization or the 1st-level analysis, this error has been propagated to the functional data as well. Consequently, any results that we generate from this analysis will be meaningless.

Setting the Origin
******************

The problem is not in the data itself; nor is it an error in the script. Instead, it is a problem with the initial alignment of the anatomical image, which causes the later preprocessing steps to fail. If you have read the other walkthroughs, you may recall from the :ref:`AFNI tutorial 07_AFNI_Checking_Preprocessing>` that an anatomical image sometimes requires a nudge or a push for a better initial alignment with the template that is being normalized to. While AFNI and FSL have methods to automatically perform these adjustments, SPM requires us to manually set the origin (i.e., values of 0,0,0 for the x-, y-, and z-dimensions). Specifically, we need to set the origin at the **anterior commissure**, a bundle of white matter fibers that connect the anterior lobes of the brain.

To do this, click on the Display button and load the raw anatomical image. (You may need to use Matlab's ``gunzip`` command to decompress the original gzipped image before you can select it from the Display GUI.) Note that if you click on the anterior commissure, the coordinates 
