.. _TBSS_Introduction:

=====================================================
Introduction to Tract-Based Spatial Statistics (TBSS)
=====================================================

.. note::

   This page is still under construction. Check back soon!

.. figure:: 

   Example_MRtrix_Output.png

---------

What is TBSS?
*************


MRtrix is a software package for analyzing diffusion data. One of the notable advantages of MRtrix over tensor-fitting techniques is their method of **constrained spherical deconvolution**, or CSD; this method deconvolves the diffusion signal in each voxel into a series of overlapping fiber bundles. This reduces the problem of crossing fibers that can be a confound when fitting a tensor.

In addition to a library of commands created by the MRtrix team, the software also has wrappers for commands used with FSL: in particular, the commands ``topup`` and ``eddy``. If you haven't already, download and install the fMRI software package :doc:`FSL </andrewjahn/AndysBrainBook/edit/master/docs/installation/fsl_mac_install>`.

.. note::

   This course is based on the steps outlined in the `MRtrix documentation <https://mrtrix.readthedocs.io/en/latest/index.html>`__, especially the "DWI Pre-Processing" and "Constrained Spherical Deconvolution" chapters. Many of the steps and explanations are derived from Marlene Tahedl's excellent `BATMAN tutorial <https://osf.io/ht7zv/>`__, and in many places I use her file notation. I would also like to thank John Plass of the David Brang lab at the University of Michigan for sharing his scripts with me and answering my questions. 


Goals of This Course
*****************

This course will show you how to analyze 


.. toctree::
   :maxdepth: 1
   :caption: Preprocessing Steps
   
   TBSS_Course/TBSS_01_Download_Install
   TBSS_Course/TBSS_02_DataDownload
   TBSS_Course/TBSS_03_DataFormats
   TBSS_Course/TBSS_04_Preprocessing
   TBSS_Course/TBSS_05_BasisFunctions
   TBSS_Course/TBSS_06_TissueBoundary
   TBSS_Course/TBSS_07_Streamlines
   TBSS_Course/TBSS_08_Connectome
   TBSS_Course/TBSS_09_Scripting
