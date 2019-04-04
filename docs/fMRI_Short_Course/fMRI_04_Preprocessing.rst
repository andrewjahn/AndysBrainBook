.. _fMRI_04_Preprocessing.rst


=============
fMRI Tutorial #4: Preprocessing
=============


.. note::
  This chapter is much longer than the other chapters; to make it easier to read and to find what you need it is divided into sections detailing each preprocessing step. Many of the examples are run from the ``Flanker/sub-08`` directory; I recommend navigating to that directory with your Terminal before reading the rest of the chapter.

Overview
-------------

Now that we're familiar with where our data is located and what it looks like, we will do the first step of fMRI analysis: **Preprocessing**.

Think of preprocessing as cleaning up images. When you take a photo with a camera, for example, there are several things you can do to make the image look better:

* Remove red eye;
* Increase color saturation;
* Remove shadows.

.. figure:: Before_After_Editing.png

  A picture we take with a camera may be dark, blurry, or noisy (left panel). After cleaning up the image by enhancing contrast, reducing blur, and increasing brightness, we end up with a more defined and clearer picture.

Similarly, when we preprocess fMRI data we are cleaning up the 3-dimensional images that we acquire every TR. An fMRI volume contains not only the signal that we are interested in - changes in oxygenated blood - but also signals that we are not interested in, such as head motion, random drifts, breathing, and heartbeats. We call these other signals **noise**, since we want to separate them from the signal that we are interested in. Some of these can be regressed out of the data by modeling them (discussed later), whereas others can be reduced or removed by preprocessing.


Preprocessing Steps
--------------

The major preprocessing steps are:

* Brain extraction (or "skull stripping")
* Motion correction
* Slice timing correction
* Smoothing
* Temporal Filtering
* Registration
* Normalization


.. note::
  Different software packages will do these steps in slightly different order - for example, FSL will normalize the statistical maps after the model has been fit. There are also analyses which omit certain steps - for example, some people who do multi-voxel pattern analyses don't smooth their data. In any case, the list above represents the most common steps that are performed on a typical dataset.
  
  
We use the term **preprocessing** because we are trying to clean up the images as best we can before fitting a model to the data. Once it has been processed, we can fit a statistical model and make claims about which conditions lead to changes in oxygenated blood.


Brain Extraction (or "skull stripping")
^^^^^^^^^^

Since fMRI studies focus on brain tissue, our first step is to remove the skull and non-brain areas from the image. FSL has a tool for this called **bet**, or the Brain Extraction Tool. It is the first button listed on the FSL GUI (window A, in the figure below). If you click on this button, another window opens that allows you to specify the Input image to skullstrip and the Output image that has been skullstripped (B), and an expandable sub-window that allows you to specify advanced options (C).

.. figure:: FSL_BET_GUI.png


.. note::
  For BET and many of the other FSL tools, you are required to specify an input image and an output image: Some operation is operation on the input image (skullstripping, for example) and the output image is the result of that operation. Usually the other options are set to defaults that you can override.


