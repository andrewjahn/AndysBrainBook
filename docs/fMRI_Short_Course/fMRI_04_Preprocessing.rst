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

Similarly, when we preprocess fMRI data we are cleaning up the 3-dimensional images that we acquire every :ref:`TR <Repetition_Time>`. An fMRI volume contains not only the signal that we are interested in - changes in oxygenated blood - but also signals that we are not interested in, such as head motion, random drifts, breathing, and heartbeats. We call these other signals **noise**, since we want to separate them from the signal that we are interested in. Some of these can be regressed out of the data by modeling them (discussed later), whereas others can be reduced or removed by preprocessing.


Preprocessing Steps
--------------

The major preprocessing steps are:

* Brain extraction (or "skull stripping")
* Motion correction
* Slice timing correction
* Smoothing
* Registration and Normalization


.. note::
  Different software packages will do these steps in slightly different order - for example, FSL will normalize the statistical maps after the model has been fit. There are also analyses which omit certain steps - for example, some people who do multi-voxel pattern analyses don't smooth their data. In any case, the list above represents the most common steps that are performed on a typical dataset.
  
  
We use the term **preprocessing** because we are trying to clean up the images as best we can before fitting a model to the data. Once it has been processed, we can fit a statistical model and make claims about which conditions lead to changes in oxygenated blood.


.. toctree::
   :maxdepth: 1
   :caption: Preprocessing Steps

   Preprocessing/Skull_Stripping
   Preprocessing/FEAT_GUI
   Preprocessing/Motion_Correction
   Preprocessing/Slice_Timing_Correction
   Preprocessing/Smoothing
   Preprocessing/Registration_Normalization



-------------

Motion Correction
^^^^^^^^^^

If you've ever tried to take a photo of a moving object, usually the image is blurry. If the object is motionless, by contrast, you will get a much clearer and sharply defined image.


.. figure:: Hand_Motion.png

  A moving target leads to a blurry image (Left), whereas a stationary target leads to a more clearly defined image (Right). 
  
The concept is the same when we take three-dimensional pictures of the brain. If the subject is moving, the images will look blurry; if he is still, the images will look more defined. But that's not all: If the subject moves a lot, we also risk measuring signal from a voxel that moves. We are then in danger of measuring signal from the voxel for part of the experiment and, after he moves, from a different region or tissue type. [Note: Need to expand on this, give a clear illustration of why this is a problem.]

Lastly, motion can introduce confounds into the imaging data because motion generates signal. If the subject moves every time in response to a stimulus - for example, if he jerks his head every time he feels an electrical shock - then it can become impossible to distinguish whether the signal we are measuring is in response to the stimulus, or because of the movement.

One way to "undo" these motions is through **rigid-body transformations**. To illustrate this, pick up a nearby object: a phone or a coffee cup, for example. Place it in front of you and mentally mark where it is. This is the **reference point**. Then move the object an inch to the left. This is called a **translation**, which means any movement to the left or right, forward or back, up or down. If you want the object to come back to where it started, you would simply move it an inch to the right. Similarly, if you rotated the object to the left or right, you could undo that by rotating it an equal amount in the opposite direction. These are called **rotations**, and like translations, they have three **degrees of freedom**, or ways that they can move: around the x-axis (also called **pitch**, or tilting forwards and backwards), around the y-axis (also known as **roll**, or tilting to the left and right), and around the z-axis (or **yaw**, as you would when shaking your head "no").

.. figure:: RigidBodyDemonstration.gif

We do the same procedure with our volumes. Instead of the reference point we used in the example above, let's call the first volume in our time-series the **reference volume**. If at some point during the scan our subject moves his head an inch to the left, we can detect that movement and undo it by moving that volume an inch to the right. The goal is to detect movements in any of the volumes and **realign** those volumes to the reference volume.

.. figure:: MotionCorrectionExample.gif

  The reference volume is typically the first volume of the time-series. If during the scan the subject moves to the right, that motion can be "undone" with respect to the reference volume by an equal and opposite movement to the left.
  
In the FEAT GUI, motion correction is specified in the ``Pre-stats`` tab. FEAT's default is to use FSL's MCFLIRT tool, which you can see in the dropdown menu. You have the option to turn off motion correction, but unless you have a reason to do that, leave it as it is.

.. figure:: FEAT_MCFLIRT.png
  :scale: 50

Slice-Timing Correction
^^^^^^^^^^

Unlike a photograph, in which the entire picture is taken in a single moment, an fMRI volume is acquired in slices. Each of these slices takes time to acquire - anywhere from tens to hundreds of milliseconds. As you'll see later on, when we model the data at each voxel we assume that all of the slices were acquired simultaneously. To make this assumption valid, the :ref:`time-series <Time_Series>` 


Smoothing
^^^^^^^^^^


Registration and Normalization
^^^^^^^^^^

Overview
***************

The last part of preprocessing is **Registration** and **Normalization**. These two steps, although distinct, are packaged together as a single step in the FEAT GUI's ``Registration`` tab. Although most people's brains are similar - everyone has a cingulate gyrus and a corpus callosum, for instance - there are also differences in brain size and shape. As a consequence, if we want to do a group analysis we need to ensure that each voxel for each subject corresponds to the same part of the brain. If we are measuring a voxel in the visual cortex, for example, we would want to make sure that every subject's visual cortex is in alignment with each other.

This is done by **Registering** and **Normalizing** the images. Just as you would fold clothes to fit them inside of a suitcase, each brain needs to be transformed to have the same size, shape, and dimensions. We do this by normalizing (or **warping**) to a **template**. A template is a brain that has standard dimensions and coordinates - standard, because most researchers have agreed to use them when reporting their results. That way, if you warp your brains to that template and find an effect at coordinates X=3, Y=20, Z=42, someone else who has warped to the same template can check their results against yours. These dimensions and coordinates of the template brain are also referred to as **standardized space**.

.. figure:: MNI_Template.png

  An example of a commonly used template, the :ref:`MNI152 brain <MNI>`. This is an average of 152 healthy adult brains, which represent the population that most studies draw from. If you are studying another population - such as children or the elderly, for example - consider using a template created from representatives of that population. (Question: Why is this template blurry? Review the previous chapter on smoothing for a hint.)
  
  
Affine Transformations
****************

To warp the images to a template, we will use an **affine transformation**. This is similar to the rigid-body transformation described above in Motion Correction, but it adds two more transformations: zooms and shears. Whereas translations and rotations are easy enough to do with an everyday object such as a pen, zooms and shears are more unusual - Zooms either shrink or enlarge the image, while shears take the diagonally opposite corners of the image and stretch them away from each other. The animation below summarizes these four types of **linear transformations**.

.. figure:: AffineTransformations.gif

.. note:: As with rigid-body transformations, zooms and shears each have three degrees of freedom: You can zoom or shear an image along the x-, y-, or z-axis. In total, then, affine transformations have twelve degrees of freedom. These are also called linear transformations because a transformation applied in one direction along an axis is accompanied by a transformation of equal magnitude in the opposite direction. A translation of one millimeter *to* the left, for example, implies that the image has been moved one millimeter *from* the right. Likewise, an enlargement of one millimeter along the positive dimension of the z-axis means that the image is enlarged in both directions simultaneously. Transformations without these constraints are called **nonlinear transformations**. For example, a nonlinear transformation can enlarge the image in one direction while shrinking in the other direction, like squeezing a sponge. These types of transformations will be discussed later.

Registration and Normalization
***************

Recall that we have both anatomical and functional images in our dataset. Our goal is to warp the functional images to the template so that we can do a group-level analysis across all of our subjects. Although it may seem reasonable to simply warp the functional images directly to the template, in practice that doesn't work very well - the images are low-resolution, and therefore less likely to match up with the anatomical details of the template. The anatomical image is a better candidate.

Although this may not seem to help us towards our goal, in fact warping the anatomical image can assist with bringing the functional images into standardized space. Remember that the anatomical and functional scans are typically acquired in the same session, and that the subject's head moves little, if at all, between the scans. If we have already normalized our anatomical image to a template and recorded what transformations were done, we can apply the same transformations to the functional images - provided they are in the same place as the anatomical image.

This alignment between the functional and anatomical images is called **Registration**. Most registration algorithms use the following steps:

1. Assume that the functional and anatomical images are in roughly the same location. If they are not, align the outlines of the images.

2. Take advantage of the fact that the anatomical and functional images have different contrast weightings - that is, areas where the image is dark on the anatomical image (such as cerebrospinal fluid) will appear bright on the functional image, and vice versa. This is called **mutual information**. The registration algorithm moves the images around to test different overlays of the anatomical and functional images, matching the bright voxels on one image with the dark voxels of another image, and the dark with the bright, until it finds a match that cannot be improved upon.

3. Once the best match has been found, then the same transformations that were used to warp the anatomical image to the template are applied to the functional images.


.. figure:: Registraton_Normalization_Demo.gif

Registration and Normalization is the last step of the preprocessing pipeline for a single subject. Now that the functional images have been fully preprocessed, we are ready to begin the next step of our analysis: fitting the General Linear Model.


.. note::

  Click the ``Next`` button to see a video demonstrating how to apply these preprocessing steps using FSL's FEAT.
