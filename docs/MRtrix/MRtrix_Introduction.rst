.. _MRtrix_Introduction:

======================
Introduction to MRtrix
======================

.. figure:: 

   Example_MRtrix_Output.png

---------

What is MRtrix?
***************


MRtrix is a software package for analyzing diffusion data. One of the notable advantages of MRtrix over tensor-fitting techniques is their method of **constrained spherical deconvolution**, or CSD; this method deconvolves the diffusion signal in each voxel into a series of overlapping fiber bundles. This reduces the problem of crossing fibers that can be a confound when fitting a tensor.

In addition to a library of commands created by the MRtrix team, the software also has wrappers for commands used with FSL: in particular, the commands ``topup`` and ``eddy``. If you haven't already, download and install the fMRI software package :doc:`FSL </andrewjahn/AndysBrainBook/edit/master/docs/installation/fsl_mac_install>`.


Goals of This Course
*****************

This course will teach you the basics of diffusion - how it is collected, and how it is analyzed. You will learn how to do fixel-based analyses to quantify the white matter fiber density within each voxel, and how to create tractograms using probabilistic tractography. Finally, you will learn how to create connectomes and how to visualize the amount of fibers that connect distinct brain regions.


.. toctree::
   :maxdepth: 1
   :caption: Preprocessing Steps
   
   MRtrix_Course/MRtrix_01_Download_Install
   MRtrix_Course/MRtrix_02_DataDownload
   MRtrix_Course/MRtrix_03_DataFormats
   MRtrix_Course/MRtrix_08_Connectome
