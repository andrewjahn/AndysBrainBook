.. _ANTs_Overview:

===================================
Advanced Normalization Tools (ANTs)
===================================

-----------

ANTs is a software package for normalizing data to a template. Most of the templates provided on the ANTs website (maybe all of them?) are in MNI space. They can be downloaded from this page. 

Below are some notes for how to use ANTs:

Skull-Stripping
***************

First, the anatomical image needs to be skull-stripped. You can do this with any skull-stripping algorithm, such as FSL's BET or AFNI's 3dSkullStrip, but for now we will only use ANTs commands:

::

  antsBrainExtraction.sh -d 3 -a sub-001_T1w_202.nii.gz -e brainWithSkullTemplate.nii.gz -m brainPrior.nii.gz -o anat_Stripped.nii
  
The option "-d 3" means that it is a three-dimensional image; "-a" indicates the anatomical image to be stripped; and "-e" is used to supply a template for skull-stripping. (which ones?) "-m" will generate a brain mask, and "-o" is the label for the output.


Or, you could use 3dSkullStrip:

::

  3dSkullStrip -input sub-001_T1w_202.nii.gz -prefix anat_ss.nii -push_to_edge
  
`optiBET <https://montilab.psych.ucla.edu/fmri-wiki/optibet/>`__ from the Monti lab is also an option:

::

  ./optiBET.sh sub-001_T1w_202.nii.gz
  
  
Inspect the resulting image to make sure the skull have been removed and the brain has been preserved.


