.. _fMRIPrep_Demo_4_AdditionalPreproc:

fMRIPrep Tutorial #4: Additional Preprocessing Steps
====================================================

---------

Overview
********

Although fMRIPrep runs your data through a standard preprocessing pipeline, it leaves out certain steps. For example, you may remember from the fMRI tutorials that **smoothing** was one of the preprocessing steps - often, one of the last ones.

Smoothing has been omitted by design; the fMRIPrep developers don't make assumptions about how you will analyze your data in your 1st-level analysis, and smoothing is an important determiner of how that will be done. For example, some MVPA studies don't do any smoothing, in order to keep the patterns of activation of nearby voxels as distinct as possible, which in turn leads to better classification. Even for fMRI studies, the amount of smoothing is a matter of taste, and experiments that are focused on larger cortical areas will probably use larger smoothing kernels.

In order to make a valid comparison with the **sub-08** data that we analyzed in the AFNI tutorial, we will run two final preprocessing steps: :ref:`smoothing 05_AFNI_Smoothing>` and :ref:`scaling <06_AFNI_Masking_Scaling>`.
