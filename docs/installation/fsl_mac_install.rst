.. _fsl_mac_install:

Overview
==================


In order to analyze fMRI data, you will need to download an fMRI analysis package. The three most popular are:

* `SPM <https://www.fil.ion.ucl.ac.uk/spm/>`__ (Statistical Parametric Mapping, created by University College London)
* `FSL <https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FSL>`__ (FMRIB Software Library, created by the University of Oxford)
* `AFNI <https://afni.nimh.nih.gov/>`__ (Analysis of Functional Neuroimages, created by the National Institutes of Health, USA)


Most laboratories use one of these three packages to analyze fMRI data. Each package is maintained by a team of professionals, and each is updated at least every few years or so. This course will focus on analyzing an online dataset using FSL; in the future, tutorials will be added for SPM and AFNI.



Installing FSL
-------------

FSL can be installed on different operating systems, such as Windows, Macintosh, and Linux. The Windows version requires a virtual machine to run FSL. This course will assume that you have a Macintosh or Linux operating system, and the tutorials will be recorded using an iMac desktop.

The instructions for installing FSL on different operating systems can be found `here <https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FslInstallation>`__.
The instructions are detailed and will not be repeated here; but for those who are using a Macintosh computer, 
a screencast can be found here to walk you through the installation and setup: 
`FSL Installation Video Walkthrough <https://youtu.be/E9FwDCYAto8?t=16>`__.


.. warning::

  You may come across the following error when running fslinstaller.py: [FAILED] [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:661). In that case, try running the installer by specifying the absolute path to your python command, e.g.: ``/usr/bin/python fslinstaller.py``.


Next Steps
-----------

If you have never used a command line before, click on the Next button to begin the Unix tutorials. If you do feel comfortable using Unix, then you can skip to the :ref:`fMRI Short Course <fMRI_Intro>`.


