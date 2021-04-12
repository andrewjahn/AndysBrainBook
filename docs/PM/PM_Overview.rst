.. _PM_Overview:

Parametric Modulation in SPM, FSL, and AFNI
===========================================

Overview
********

If you have completed the previous tutorials on SPM, FSL, or AFNI, you are able to create general linear models (GLMs) for an fMRI task study. You may have noticed that as single parameter estimate was calculated for each regressor in your model; these parameter estimates were then calcualted for each voxel for each subject, and then used for a group-level analysis to determine where there were significant differences between parameter estimates.

While many experiments use this approach, there are other scenarios in which we examine not just the BOLD response to the condition itself, but the BOLD response to different aspects of the stimulus. For example, assume that we have a condition in which a light is shown for a few seconds, and then switched off. During certain trials the light is relatively weak, while during other trials, the light is relatively strong. If we had a measurement of the light intensity, in candelas, we could determine whether the BOLD response covaries with the light intensity.

This covariation is called **parametric modulation**; in other words, does the BOLD signal seem to increase as the intensity of the stimulus increases, and decrease as the intensity decreases? This is not just restricted to the intensity of a light stimulus. For example, social psychology researchers may require the participant to rate a particular image on how aversive it is, how attractive it is, or among other scales.

In order to illustrate how parametric modulation works, we will be using a gambling dataset from `Tom et al., 2007 <https://science.sciencemag.org/content/sci/315/5811/515.full.pdf>`__. In the following chapters we will learn how to download the dataset and how to analyze it in each of the major fMRI software packages: SPM, FSL, and AFNI.

.. toctree::
   :maxdepth: 1
   :caption: Start to Finish Analysis with SPM

    PM_Short_Course/PM_01_DataDownload
    PM_Short_Course/PM_02_SPM
    PM_Short_Course/PM_03_FSL
    PM_Short_Course/PM_04_AFNI
