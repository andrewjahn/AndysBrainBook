.. _Smoothing.rst


.. note::

  This chapter is still under construction; please check back soon.
  
Smoothing
=============

Why Smooth?
-----------

It is common to smooth the functional data, or replace the signal at each voxel with a weighted average of that voxel's neighbors. This may seem strange at first - why would we want to make the images blurrier than they already are?

It is true that smoothing does decrease the spatial resolution of your functional data, and we don't want less resolution. But there are benefits to smoothing as well, which can outweigh these drawbacks. For example, we know that our functional data contain a lot of noise, and that the noise is frequently greater than the signal. By averaging over nearby voxels we can cancel out the noise and enhance the signal.


.. figure:: Smoothing_Demo.gif

.. (Talk about an example here of how averaging works to give rise to a true signal? I'm thinking about the example in which ten students are asked the population of the city they are in; no individual estimate is right, but averaged together it is pretty close to the true population.)

There is another benefit to smoothing. As you will see in the next chapter, our goal is to **normalize** every subject's brain to a template brain which has standardized coordinates. To make this normalization as smooth as possible, 



