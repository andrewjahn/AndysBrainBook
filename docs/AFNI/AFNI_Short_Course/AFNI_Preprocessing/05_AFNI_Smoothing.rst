.. _05_AFNI_Smoothing:

==================
Chapter 5: Smoothing
==================


------

Why Smooth?
-----------

It is common to **smooth** the functional data, or replace the signal at each voxel with a weighted average of that voxel's neighbors. This may seem strange at first - why would we want to make the images blurrier than they already are?

It is true that smoothing does decrease the spatial resolution of your functional data, and we don't want less resolution. But there are benefits to smoothing as well, and these benefits can outweigh the drawbacks. For example, we know that fMRI data contain a lot of noise, and that the noise is frequently greater than the signal. By averaging over nearby voxels we can cancel out the noise and enhance the signal.


.. figure:: Smoothing_Demo.gif

  In this animation, two different smoothing kernels (4mm and 10mm) are applied to an fMRI scan. Notice that as we use larger smoothing kernels, the images become blurrier and the anatomical details become less distinct. Also note that, for the sake of simplicity, this animation uses a 2D slice of the brain to demonstrate this preprocessing step. In actual fMRI data, the kernel would be applied in all three dimensions.

.. (Talk about an example here of how averaging works to give rise to a true signal? I'm thinking about the example in which ten students are asked the population of the city they are in; no individual estimate is right, but averaged together it is pretty close to the true population.)

There is another benefit to smoothing. As you will see in the next chapter, our goal is to **normalize** every subject's brain to a template brain which has standardized coordinates. Click on the Next button to learn more about Normalization, and how smoothing can help improve statistical power.



