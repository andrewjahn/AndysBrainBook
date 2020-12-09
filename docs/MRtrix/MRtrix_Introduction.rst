.. _MRtrix_Introduction:

======================
Introduction to MRtrix
======================

.. note::

   Update 12.09.2020: All chapters completed except for group-level analysis; this last part should be done in January 2021.

.. figure:: 

   Example_MRtrix_Output.png

---------

What is MRtrix?
***************


MRtrix is a software package for analyzing diffusion data. One of the notable advantages of MRtrix over tensor-fitting techniques is their method of **constrained spherical deconvolution**, or CSD; this method deconvolves the diffusion signal in each voxel into a series of overlapping fiber bundles. This reduces the problem of crossing fibers that can be a confound when fitting a tensor.

In addition to a library of commands created by the MRtrix team, the software also has wrappers for commands used with FSL: in particular, the commands ``topup`` and ``eddy``. If you haven't already, download and install the fMRI software package :doc:`FSL </andrewjahn/AndysBrainBook/edit/master/docs/installation/fsl_mac_install>`.

.. note::

   This course is based on the steps outlined in the `MRtrix documentation <https://mrtrix.readthedocs.io/en/latest/index.html>`__, especially the "DWI Pre-Processing" and "Constrained Spherical Deconvolution" chapters. Many of the steps and explanations are derived from Marlene Tahedl's excellent `BATMAN tutorial <https://osf.io/ht7zv/>`__, and in many places I use her file notation. I would also like to thank John Plass of the David Brang lab at the University of Michigan for sharing his scripts with me and answering my questions. 


Goals of This Course
*****************

This course will teach you the basics of diffusion - how it is collected, and how it is analyzed. You will learn how to do fixel-based analyses to quantify the white matter fiber density within each voxel, and how to create tractograms using probabilistic tractography. Finally, you will learn how to create connectomes and how to visualize the amount of fibers that connect distinct brain regions.


.. toctree::
   :maxdepth: 1
   :caption: Preprocessing Steps
   
   MRtrix_Course/MRtrix_00_Diffusion_Overview
   MRtrix_Course/MRtrix_01_Download_Install
   MRtrix_Course/MRtrix_02_DataDownload
   MRtrix_Course/MRtrix_03_DataFormats
   MRtrix_Course/MRtrix_04_Preprocessing
   MRtrix_Course/MRtrix_05_BasisFunctions
   MRtrix_Course/MRtrix_06_TissueBoundary
   MRtrix_Course/MRtrix_07_Streamlines
   MRtrix_Course/MRtrix_08_Connectome
   MRtrix_Course/MRtrix_09_Scripting
