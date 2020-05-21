.. _FS_02_DownloadInstall:

===================================================
FreeSurfer Tutorial #2: How to Download and Install
===================================================

-----------

Downloading FreeSurfer
**********************

FreeSurfer's `installation page <https://surfer.nmr.mgh.harvard.edu/fswiki/DownloadAndInstall>`__ provides detailed instructions on how to download and install the FreeSurfer package. This includes registering for a license, which you will need to place in the FreeSurfer directory in order to use the software.

When you have finished downloading and installing the package, you will use :ref:`freeview <FS_06_Freeview>` to check whether the software installed successfully. We will cover more advanced features of freeview in a later tutorial.

.. note::

  FreeSurfer is designed for Unix and Macintosh operating systems. It is possible that there is a way to install FreeSurfer on Windows by using a Unix emulator, but there is no systematic documentation on the FreeSurfer website demonstrating how to do it. See `here <https://surfer.nmr.mgh.harvard.edu/fswiki/WindowsSupport>`__ for the basics of a Windows installation.
  
  
Downloading the Example Dataset
*******************************

For the rest of our tutorials, we will be using a `dataset from openneuro.org <https://openneuro.org/datasets/ds000174/versions/1.0.1>`__ that contains anatomical scans from cannabis users and controls. It is a longitudinal study with two timepoints - one baseline scan and one follow-up scan - and individual difference measures such as age and sex. This will enable us to do several different types of analyses, such as group comparisons, longitudinal analyses, and individual difference correlations with grey matter measurements. Download the dataset and unpack it by double-clicking on the file, and then rename the folder by typing ``mv ds000174-1.0.1 Cannabis``.

The dataset contains one group of 20 cannabis smokers and one group of 22 controls (i.e., individuals who have never smoked cannabis). Subjects whose numerical ID begins with "1" belong to the cannabis group, and subjects whose numerical ID begins with a "2" or "3" belong to the control group. For example, sub-108 would belong to the cannabis group, and sub-320 would belong to the control group.

Each subject's directory contains two sub-directories labeled ``ses-BL``, indicating the Baseline session, and ``ses-FU``, indicating the Follow-Up session. Within each of these folders is another sub-directory called ``anat``, which contains the anatomical scan for that session. To explore how the dataset is organized, navigate to the Cannabis directory and type the following command:

::

  ls sub-112/ses-BL/anat
  
.. warning::

  When testing your installation by running the mri_convert command, you may get an error like this: ``mir_convert.bin: error while loading shared libraries: libgomp.so.1: cannot open shared object file``. In that case, try manually installing libgomp with this command: ``sudo apt-get install libgomp1``. Also, if you receive this error: ``/home/$USERNAME/freesurfer/license.txt exists but you do not have read permission``, try typing: ``chmod a+r /home/$USERNAME/freesurfer/license.txt``.

Next Steps
**********

Now that you have downloaded FreeSurfer and some example data, you are ready to learn about FreeSurfer's **recon-all** command, which will process a dataset from start to finish. Click on the ``Next`` button to learn more about how it works. 

-------
  
Video
*****

For a video walkthrough showing you how to download and install FreeSurfer, click `here <https://www.youtube.com/watch?v=BSQUVktXTzo>`__.
