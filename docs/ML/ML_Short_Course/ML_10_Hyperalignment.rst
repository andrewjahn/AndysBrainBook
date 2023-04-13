.. _ML_10_Hyperalignment:

=============================================
Machine Learning Tutorial #10: Hyperalignment
=============================================

.. note::

  This section is still under construction; check back soon!

---------------

Overview
********

One of the newer techniques in machine learning is **hyperalignment**, developed in James Haxby's lab at Dartmouth. Instead of tranining a classifier on a pattern of voxels within a subject's 3-D volume or 2-D surface, hyperalignment instead transforms a given set of voxels to a higher dimensional **information space**, which has as many dimensions as there are voxels in the region you are analyzing. For example, instead of a three dimensional cube of voxels, this hyperspace would have one axis per voxel, with the activity recorded in the voxel determining how far along the axis this voxel will be located. Another subject's data is then transformed to match the other subject's data in hyperspace, using a rigid-body transformation called a **Procrustes Transformation**. Once the best alignment is found, this is performed for each additional subject, using the average of the previous subjects as a target. When a new dataset is acquired, it is then compared to this hyperspace to see which condition the data best fit.

The main benefit of hyperalignment is a significant improvement in between-subject classification. Traditional MVPA techniques 


Useful Links
************

There is an `excellent introduction to Hyperalignment <https://github.com/jwparks/Hyperalignment_tutorial/blob/main/Tutorial.ipynb>`__ by Jiwoong Park and Sooahn Lee, using Python.

The `Neuroimaging and Data Science <https://neuroimaging-data-science.org/root.html>`__ tutorial by Tal Yarkoni and Ariel Rokem is a good place to start not only for hyperalignment, but also to learn how to use Python for Neuroimaging analysis.

`Luke Chang's lab <https://naturalistic-data.org/content/Functional_Alignment.html>`__ at Dartmouth has a tutorial about how to use hyperalignment to analyze a dataset in which subjects watched a TV show.
