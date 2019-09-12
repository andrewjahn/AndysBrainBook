.. _AFNI_09_SurfaceAnalysis:

==================================================
AFNI Tutorial #9: Surface-Based Analysis with SUMA
==================================================

Our analyses so far have been **volume-based** - that is, we have preprocessed and calculated statistics for each voxel of a three-dimensional cube. Our goal in this chapter is to do the preprocessing and statistical modeling on a two-dimensional **surface** of the cortex. This gives you several advantages, including:

1. The ability to visualize activity along the surfaces of the gyri and sulci, which gives you a better picture of where the activity is localized;
2. This also avoids the :ref:`partial voluming problem <FS_01_BasicTerms>` of a voxel that encompasses the edges of multiple gyri, making it impossible to determine which part of the cortex the activity comes from;
3. The ability to use larger smoothing kernels to increase your signal-to-noise ratio.

We will run this analysis with **SUMA**, a package that comes with your AFNI installation. Technically, SUMA is a separate package that can be run on its own, but we will be using it in conjunction with the AFNI viewer and with AFNI commands.

.. figure:: SUMA.png

  SUMA! Photo taken from the `AFNI website <https://afni.nimh.nih.gov/pub/dist/doc/SUMA/suma/SUMA_do1.htm>`__.

Before going on, review the :ref:`FreeSurfer tutorials <FreeSurfer_Introduction>` to make sure you have FreeSurfer installed. You will also want to install the command `parallel <FS_04_ReconAllParallel>` to speed up your analysis. 


.. toctree::
   :maxdepth: 1
   :caption: Start-to-Finish fMRI Analysis with SUMA

   SUMA/SUMA_01_ReconSurface
   SUMA/SUMA_02_MakeSpec_AlignExp
   SUMA/SUMA_03_AnalysisOnTheSurface
   SUMA/SUMA_04_GroupAnalysisOnTheSurface
   SUMA/SUMA_05_ROIAnalysisOnTheSurface
   SUMA/SUMA_06_Summary
