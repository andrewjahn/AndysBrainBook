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


Analyzing Data with Hyperalignment
**********************************

To illustrate how to perform hyperalignment, we will use the Sherlock dataset, which is available `here <https://openneuro.org/datasets/ds001132/versions/1.0.0>`__ on OpenNeuro. The rest of this tutorial will closely follow the steps outlined in the Luke Change walkthrough listed above; he deserves the credit for explaining how to do the analysis. What follows is mostly my paraphrasing of his work; my goal is to try and explain it in my own words, and to use this as a teaching tool for my own purposes. Also note that you can either start with the raw data hosted on OpenNeuro (about 16 gigabytes), or use the fully preprocessed data listed on Luke Chang's website. For the rest of this tutorial, we will use the fully preprocessed data.

To begin, we will download parts of the dataset using `DataLad <https://www.datalad.org/>`__, a command for downloading datasets. It is similar to commands such as ``wget`` and ``curl``, but allows for more sophisticated downloads.

For example, the fully preprocessed Sherlock dataset as a whole is over a hundred gigabytes; if we don't have enough space on our local machine to analyze it, we can instead download just a few subjects using DataLad. For Macintosh Operating Systems, I recommend acquiring DataLad with `HomeBrew <https://brew.sh/>`__. Once DataLad is installed, you can begin downloading the Sherlock dataset by typing:

::

  datalad install https://gin.g-node.org/ljchang/Sherlock

You can see how large the entire dataset would be by navigating into the Sherlock folder and typing:

::

  datalad status --annex
  
Which returns a total size of 109.0 gigabytes. If we want to download individual files, we can navigate to the ``stimuli`` directory, for example, and type:

::

    datalad get *
    
Which will download all of the files in that directory. Do the same for the files in the ``onsets`` directory, and then navigate into the ``fmriprep`` directory. This contains the fully preprocessed data analyzed with fMRIPrep. Let's say that we just want to download the first four subjects; we can do so by typing:

::

  datalad get sub-01 sub-02 sub-03 sub-04
  
Depending on your connection speed, this can take several hours.


Setting Up Your Conda Environment
*********************************

Conda is a **virtual environment** which can install packages into a partition separate from the rest of your computer. For example, within this environment we can install a version of Python that is different from the default one installed on your machine; this way, we can avoid any version issues and any configuration problems that might arise if all of these different software packages were installed on the same machine.

We will use conda to create a virtual environment for the rest of the analyses in this tutorial. First, download `anaconda` (a more complete suite of options for using conda commands) `here <https://www.anaconda.com/download/>`__. When you have finished downloading it, open a terminal and type ``conda init bash``. Then, create a new virtual environment by typing:

::

   conda create -n naturalistic python=3.7 anaconda
   
   
.. note::

  If you receive an error saying ``PackagesNotFoundError: The following packages are not available from current channels: - python=3.7``, that could be due to python version 3.7 not being available by default on newer Apple models, as of this writing circa 2021-2023. See `this thread https://stackoverflow.com/questions/70205633/cannot-install-python-3-7-on-osx-arm64>`__ for an explanation of how to get around this error. In that case, you can use a more recent version of python, such as 3.9:
  
  ::
  
    conda create -n naturalistic python=3.9 anaconda
