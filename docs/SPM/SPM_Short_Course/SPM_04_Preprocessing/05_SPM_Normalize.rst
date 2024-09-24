.. _05_SPM_Normalize:

========================
Chapter 5: Normalization
========================

---------------

Performing Normalization in SPM
*******************************

After the anatomical image has been segmented, we can use those segmentations to **normalize** the image. From the SPM GUI, click on ``Normalize (Write)``, click on the ``Data`` field in the Batch Editor, and create a new Subject. Select the Deformation Field that you created in the ``anat`` directory during Segmentation (the file should be called "y_rsub-08_T1w.nii"), and for ``Images to Write`` select all of the realigned and slice-time corrected images. (You can do this more efficiently by typing ``^ar.*`` in the Filter field, and entering ``1:146`` in the Frames field.)

In the ``Writing Options`` section, you can change the voxel resolution of the images that are warped. The default of 2x2x2 will create higher-resolution images, but the files will take up more space on your computer. If you want to create smaller files with lower resolution, change this to ``3 3 3``.

.. note::

  You can also select the coregistered anatomical image as an image to normalize, which may be useful if you want to visualize the individual subject's results on their own anatomy. For the purposes of this tutorial, we will only be visualizing the results on a template brain, so there is no need to normalize the anatomical image.


Checking the Output
*******************

Once the functional images have been normalized, check the output to make sure that there were no errors. From the SPM GUI, click on ``Check Reg``, and select one of your functional volumes that has a ``w`` prepended to it (indicating that it has been **warped** - that is, normalized). For the second image, go to the directory ``spm12/canonical`` and select any of the ``T1`` images - either avg152T1.nii, avg305T1.nii, or single_subj_T1.nii. As with :ref:`Coregistration <03_SPM_Coregistration>`, check to make sure that both the outlines of the brains and the internal structures are well-aligned.

.. note::

  The template ``single_subj_T1.nii`` will have the clearest spatial resolution - i.e., you will be able to see each of the individual gyri and sulci. However, visualizing your results on this template may be slightly misleading, since each subject's anatomy has been warped and blurred; activation that appears to be in a specific location on the single_subj_T1 template may not be as specific as it appears. For this reason, it is recommended to visualize your activation on one of the averaged templates, or on an average image consisting of the mean of your subject's normalized anatomical images. We will discuss this in more detail when we cover statistical modeling in the next chapter.
  
.. figure:: 04_05_CheckNormalization.gif

-----------------

Exercises
*********

1. While the most recent version of SPM's normalization is recommended, you also have the option of using an older version of normalize - one that doesn't require input from the Segmentation step. This can be found from the SPM GUI by clicking on the ``Batch`` button, and from the top of the window selecting ``SPM -> Tools -> Old Normalise: Estimate and Write``.  Click on the Data button and create a new Subject, and select the resliced anatomical image as the Source Image and all 292 volumes of your coregistered functional data as the Images to Write (i.e., those images that begin with the "r" prefix; you can select them in the file selection window by using a filter of ``^rsub``). For the Template Image, select ``T1.nii``. Show a screenshot of the output from this normalization, and comment on any noticeable differences between this normalization and the one that uses segmentation.

2. Change the ``Voxel sizes`` from [2 2 2] to [3 3 3], also changing the Filename Prefix from ``w`` to ``w_3_3_3``. Compare the spatial resolution of the two outputs. What resolution seems best to you? What are the disadvantages of using a resolution that is very small, such as [1 1 1]?

3. As with coregistration, you can select a different amount of Interpolation. The default is ``4th Degree B-Spline``, with options for higher degrees of interpolation (such as 7th Degree B-Spline), or lower degrees of interpolation - the lowest level being ``Nearest Neighbour``, which uses the value of the nearest voxel for resampling (see `this animation <https://andysbrainbook.readthedocs.io/en/latest/FrequentlyAskedQuestions/FrequentlyAskedQuestions.html#resampling>`__ for an illustration; you may need to scroll down the page). When would a higher degree of interpolation be desirable, and when would you want to do Nearest Neighbour interpolation? Try using both interpolation options, and compare the results.
