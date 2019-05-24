.. _fMRI_09_ROIAnalysis:

fMRI Tutorial #9: ROI Analysis
=====================

---------

Overview
********

You've just completed a group-level analysis, and identified which regions of the brain show a significant difference between the Incongruent and Congruent conditions of the experiment. For some researchers, this may be all that they want to do.

This kind of analysis is called a **whole-brain** or **exploratory** analysis. These types of analyses are useful when the experimenter doesn't have a hypothesis about where the difference may be located; the result will be used as the basis for future research.

When a large number of studies have been run about a specific topic, however, we can begin to make more specific hypotheses about where we should find our results in the brain images. For example, cognitive control has been studied for many years, and many fMRI studies have been published about it using different paradigms that compare more cognitively demanding tasks to less cognitively demanding tasks. Often, significant changes in the BOLD signal are seen in a region of the brain known as the **dorsal medial prefrontal cortex**, or dmPFC for short. For the Flanker study, then, we could restrict our analysis to this region and only extract data from voxels within that region. This is known as a **region of interest** analysis, or ROI analysis for short. A general name for an analysis in which you choose to analyze a region selected before you analyze the results is called a **confirmatory analysis**.

The whole-brain maps that we generate can hide important details about the effects that we’re studying. We may find a significant effect of incongruent-congruent, but the reason the effect is significant could be because incongruent is greater than congruent, or because congruent is much more negative than congruent, or some combination of the two. The only way to determine what is driving the effect is by ROI analysis, and this is especially important when dealing with interactions and more sophisticated designs.


Using Atlases
*******

One way to create a region for our ROI analysis is to use an **atlas**, or a map of the brain that partitions it into anatomically distinct sections.

FSL has many atlases already installed, which you can access through fsleyes. If you click on Settings -> Ortho View 1 -> Atlas Panel, it will open a new window called ``Atlases``. By default, the Harvard-Oxford Cortical and Subcortical Atlases will be loaded. You can see how the atlas partitions the brain by clicking on the ``Show/Hide`` link next to the atlas name. The voxel at the center of the crosshairs in the viewing window will be assigned a probability of belonging to a brain structure.

.. figure:: ROI_Analysis_Atlas_Example.png

  The Harvard-Oxford Cortical atlas, displayed on an MNI template brain. The Atlas window shows the probability that the voxel is located at a certain anatomical region.
  
To save one of these regions as a file to extract data from, also known as a **mask**, click on the ``Show/Hide`` link next to the region you want to use as a mask - in our example, let's say that we want to use the Paracingulate Gyrus as a mask. Clicking on the link will show that region overlaid on the brain, as well as load it as an overlay in the Overlay List window. Click on the disk icon next to the image to save it as a mask. Save it to the Flanker directory and call it ``PCG.nii``.

.. figure:: ROI_Analysis_PCG_Mask.png


.. warning::

  Your results will have the same resolution as the template you used for normalization. The default in FSL is the MNI_152_T1_2mm_brain, which has a resolution of 2x2x2mm. When you create a mask, it will have the same resolution as the template that it is overlaid on. When we extract data from the mask, the data and the mask need to have the same resolution. To avoid any errors due to different image resolutions, use the same template to create the mask that you used to normalize your data.
  

Extracting Data from an Anatomical Mask
************

Once you've created the mask, you can then extract each subject's contrast estimates from it. Although you may think that we would extract the results from the 3rd-level analysis, we actually want the ones from the 2nd-level analysis; the 3rd-level analysis is a single image with a single number at each voxel, whereas in an ROI analysis our goal is to extract the contrast estimate for each subject individually.

For the Incongruent-Congruent contrast estimate, for example, you can find each subjects' data maps in the directory ``Flanker_2ndLevel.gfeat/cope3.feat/stats``. The data maps have been calculated several different ways, including t-statistic maps, the cope images, and variance images. My preference is to extract data from the z-statistic maps, since these data have been converted to a form that is normally distributed and, in my opinion, is easier to plot and to interpret.


We will need to merge all of the z-statistic maps into a single dataset. To do this, we will use a combination of FSL commands and Unix commands. Navigate into the ``Flanker_2ndLevel.gfeat/cope3.feat/stats``, and then type the following:

::

  fslmerge -t allZstats.nii.gz `ls zstat* | sort -V`
  
This will merge all of the z-statistic images into a single dataset along the time dimension (specified with the ``-t`` option); this simply means to daisy-chain the volumes together into a single larger dataset. The first argument is what the output dataset will be called (``allZstats.nii.gz``), and the code in backticks uses an asterisk wildcard to list each file beginning with "zstat", and then sorts them numerically from smallest to largest with the ``-V`` option.

Move the allZstats.nii.gz file up three levels so that it is in the main Flanker directory (i.e., type ``mv allZstats.nii.gz ../../..``). Then use the fslmeants command to extract the data from the PCG mask:

::

  fslmeants -i allZstats.nii.gz -m PCG.nii.gz
  
This will print 26 numbers, one per subject. Each number is the contrast estimate for that subject averaged across all of the voxels in the mask. 

.. figure:: ROI_Analysis_FSLmeants_output.png

  Each number output from this command corresponds to the contrast estimate that went into the analysis. For example, the first number corresponds to the average contrast estimate for Incongruent-Congruent for sub-01, the second number is the average contrast estimate for sub-02, and so on. These numbers can be copied and pasted into a statistical software package of your choice (such as R), and then you can run a t-test on them.
  
Extracting Data from an Anatomical Mask
************

.. figure:: ROI_Analysis_Anatomical_Spherical.gif
