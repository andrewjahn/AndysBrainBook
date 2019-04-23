.. _FrequentlyAskedQuestions.rst

Frequently Asked Questions
==============

This is a list of common questions that I am asked. I have found that most questions can be organized into categories such as Resampling, Cluster Correction, Normalization, and so on. Some of these questions may eventually be folded into the fMRI Concepts section.

This list was created on April 23rd, 2019, and will be updated each week. 


Resampling
--------

Question: What is resampling? 

Answer: Resampling means changing either the resolution, dimensions, or both the resolution and dimensions of an image. A typical fMRI image is composed of voxels, which are like the pixels that make up your computer screen, but in three dimensions. For example, a common voxel size is 3 by 3 by 4 millimeters. 

.. figure: Voxel_Example.png

  An fMRI image is composed of cubes called voxels (lower right). Each of these cubes contains a single number representing the signal measured at that voxel. Voxels can either be isotropic, with dimensions of equal lengths, or anisotropic, with at least one dimension either longer or shorter than the other dimensions. One common use of resampling is to make the dimensions of the voxel either shorter or longer, which in turn creates larger or smaller voxels. Larger voxels lead to a lower-resolution image, and smaller voxels will lead to a higher-resolution image.
  

You can resample an image using several different methods. Let's start with the nearest neighbor method, which is the easiest to illustrate. Imagine that we have a 4x4 grid with a number in each square, and that we want to resample it to a 3x3 grid. In the animation below, we superimpose a 3x3 grid on the original 4x4 grid and then find which number is closest to the center of the new square. In the upper left corner, for example, the number 8 is closest to the center of the new square. As a result, the new square will now contain the value 8.

.. figure: NearestNeighbor_Example.gif



Other Questions
------

1. What is the difference between a functional and a structural image?
2. Where do the fMRI templates come from? When should one use a template other than the default?
3. What are the types of images that one can generate from the scanner, and how are they different? What questions can they answer?
  
