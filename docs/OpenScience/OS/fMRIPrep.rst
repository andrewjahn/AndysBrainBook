.. _fMRIPrep:

=============
BIDS App Tutorial #2: fMRIPrep
=============

-------------

.. note::

  This article was contributed by `Daniel Levitas <https://perceptionandneuroimaging.psych.indiana.edu/people/daniellevitas.html>`__ of the Perception and Neuroimaging Lab at Indiana University.
  
What is fMRIPrep?
*************

fMRIPrep is a BIDS App that leverage BIDS compliant dataset to perform standardized preprocessing of fMRI data. This pipeline utilizes the leading tools from different fMRI software packages (AFNI, FSL, ANTs, Freesurfer, nipype) to perform specific steps in the preprocessing. In addition, users can easily access the workflows to determine how their data were preprocssed, thus providing transparency and reproducibility. 

Within the neuroimaging community there exists a plethora of preprocessing pipelines for fMRI data -- most of which are not made publicly available. The result is that individual labs oftentimes have idiosyncratic preprocessing pipelines that can lead to issues of reproducibility.

fMRIPrep Tutorial
*****************

This tutorial will demonstrate how to install fMRIPrep and run it on a dataset. The data that we'll be using is the BIDS-ified output from the `BIDS Overview and Tutorial <https://andysbrainbook.readthedocs.io/en/latest/OpenScience/OS/BIDS_Overview.html>`__. 

