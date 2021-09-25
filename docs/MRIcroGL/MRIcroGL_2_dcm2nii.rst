.. _MRIcroGL_2_dcm2nii:

===============================================
MRIcroGL Tutorial #2: Converting DICOM to NIFTI
===============================================

Overview
--------

In recent years, NIFTI has become a standard format for neuroimaging data - the major fMRI software packages of FSL, SPM, and AFNI can all load it and generate output in NIFTI format, and it can be read by other widely-used program as well, such as FreeSurfer.

However, the raw data collected by the scanner isn't in NIFTI format. Each of the major scanner models - Siemens, Philips, and General Electric - has its own raw data format, which usually stores the data as individual slices. The raw data from a Siemens scanner, for example, is in Digital Imaging and Communications in Medicine (DICOM) format. These slices contain header information indicating which volume they belong to, and which scanning sequence they belong to, such as an anatomical, functional, or diffusion scan. When converting to NIFTI format, the slices are stacked together into individual volumes, and the volumes are concatenated together into their corresponding scan sequence. This makes the data much more compact and easier to manipulate.


Downloading a Sample Dataset
----------------------------

To demonstrate how to use MRIcroGL's DICOM to NIFTI converter, we will download a sample dataset from the Nathan Kline Institute (NKI) Rockland Sample. Click on `this link <https://fcon_1000.projects.nitrc.org/indi/pro/eNKI_RS_TRT/FrontPage.html>`__, scroll to the bottom of the page, and click on the link that says ``Download DICOM Files``. 

.. figure:: 02_DownloadData.png

This will take you to a new page, listing the subject IDs for every participant in the study. Check the box next to subject 2475376, and then click ``Download Selected``.

.. figure:: 02_DownloadData2.png

This will take you to the NITRC page - the same website that hosts the MRIcroGL software package - and you will be required to sign up for an account and log in, if you haven't already. Once you do that, the download will begin; the dataset is about two gigabytes, and will take a few minutes, depending on the speed of your Internet connection.


Converting the Data
-------------------

When the dataset has been downloaded, click on it to unzip it. Once it is uncompressed, you should see something like this in your Finder:

.. figure:: 02_DataStructure.png

This subject has several different runs of imaging data: An anatomical scan, a scan during which the subject held their breath, data collected when a visual checkerboard was shown, and so on. There are also two folders marked ``session1`` and ``session2``, which contain diffusion and resting state scans.

For now, let's focus on the folders ``anat`` and ``session1``. MRIcroGL's graphical user interface makes it easy to convert all of the data in these folders by simply clicking and dragging them onto the GUI. From the MRIcroGL menu at the top of your window, click on ``Import -> Convert DICOM to NIFTI``. You will see another window opened up that looks like this:

.. figure:: 02_DICOMtoNIFTI_GUI.png

There are several options here, which I encourage you to explore on your own. The sidebar on the left of the converter GUI contains options you can change; the window on the right shows the output when you run the converter. The first field, ``Output Filename``, by default contains the string ``%f_%p_%t_%s``. Each of these letters preceded by a percentage symbol is called a **formatting operator**, which is shorthand for 
