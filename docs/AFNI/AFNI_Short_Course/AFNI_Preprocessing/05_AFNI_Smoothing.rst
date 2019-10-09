.. _05_AFNI_Smoothing:

==================
Chapter 5: Smoothing
==================


------

Why Smooth?
-----------

It is common to **smooth** the functional data, or replace the signal at each voxel with a weighted average of that voxel's neighbors. This may seem strange at first - why would we want to make the images blurrier than they already are?

It is true that smoothing does decrease the spatial resolution of your functional data, and we don't want less resolution. But there are benefits to smoothing as well, and these benefits can outweigh the drawbacks. For example, we know that fMRI data contain a lot of noise, and that the noise is frequently greater than the signal. By averaging over nearby voxels we can cancel out the noise and enhance the signal.


.. figure:: 04_05_Smoothing_Demo.gif

  In this animation, two different smoothing kernels (4mm and 10mm) are applied to an fMRI scan. Notice that as we use larger smoothing kernels, the images become blurrier and the anatomical details become less distinct. Also note that, for the sake of simplicity, this animation uses a 2D slice of the brain to demonstrate this preprocessing step. In actual fMRI data, the kernel would be applied in all three dimensions.

.. (Talk about an example here of how averaging works to give rise to a true signal? I'm thinking about the example in which ten students are asked the population of the city they are in; no individual estimate is right, but averaged together it is pretty close to the true population.)

Smoothing is done with AFNI's ``3dmerge`` command, which you will find under the "blur" header of the proc_Flanker script (lines 216-221). Of all the preprocessing steps, this one uses the fewest lines of code:

::

  # blur each volume of each run 
  foreach run ( $runs )
    3dmerge -1blur_fwhm 4.0 -doall -prefix pb03.$subj.r$run.blur \
            pb02.$subj.r$run.volreg+tlrc
  end

The ``-1blur_fwhm`` option specifies the amount to smooth the image, in millimeters - in this case, 4mm. ``-doall`` applies this smoothing kernel to each volume in the image, and the ``-prefix`` option, as always, specifies the name of the output dataset.

The last preprocessing steps will take these smoothed images and then scale them to have a mean signal intensity of 100 - so that deviations from the mean can be measured in percent signal change. Any non-brain voxels will then be removed by a mask, and these images will be ready for statistical analysis. To see how these last two preprocessing steps are done in AFNI, click the ``Next`` button.

