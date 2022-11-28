.. _ITK-Snap_01_InfantFreeSurfer:

=======================================
ITK-Snap Tutorial #1: Infant FreeSurfer
=======================================

.. note:: This tutorial assumes that you are already familiar with the basics of FreeSurfer and its terminology; if you haven't used FreeSurfer, yet, please work through :ref:`this tutorial <FreeSurfer_Introduction>` before continuing. You will also need to download and install FSL (`https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FslInstallation <https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FslInstallation>`_) in order for infant_recon-all to work.

---------------

Because infant brains are much smaller than adult brains, the typical FreeSurfer preprocessing steps won't work properly. We will instead download a slightly different version of FreeSurfer called **Infant FreeSurfer** (or InfantFS for short), which can be download `here <https://surfer.nmr.mgh.harvard.edu/ftp/dist/freesurfer/infant/`_. Click on the distribution corresponding to your operating system; for this tutorial, I will be using the Macintosh OS version. Once you have downloaded and unzipped the file, move it to your home directory, and add the following code to your ``bash.rc`` start-up file:

:: 

  export FREESURFER_HOME=~/freesurfer
  source $FREESURFER_HOME/SetUpFreeSurfer.sh
  
Once you have done this, you should have access to the InfantFS libraries. For example, if you type the command ``infant_recon-all`` from any directory in your terminal and press enter, it should return a list of the arguments needed to run the command.


Analyzing a Subject with infant_recon-all
*****************************************

