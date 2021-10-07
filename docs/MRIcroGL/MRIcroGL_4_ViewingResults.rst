.. _MRIcroGL_4_ViewingResults:

=====================================
MRIcroGL Tutorial #4: Viewing Results
=====================================

--------

Overview
********

As part of writing your manuscript, you will want to create figures that show where your results are located. We have already seen how to view the results in the major fMRI packages, but if you like the MRIcroGL interface, you can view the results there instead.

Staying with the results from our FSL analysis, we will first want to load a template as the **underlay**, or the image on which the results are overlaid. If you click on File and then hover your cursor over ``Open Standard``, you will see a list of templates that come with MRIcroGL. Some of these are used for tutorials specific to MRIcroGL, such as the Iguana image, or any of the CT images. In this case, we will select ``mni152``, a widely-used template in neuroimaging that is in standardized space.

Next, we will want to **overlay** a results image onto the template. You can load any results you want; in this tutorial, we will load the threshold-free cluster enhancement results from the contrast Incongruent-Congruent. Click on ``File -> Add Overlay``, and select the file ``allZstats_randomise_tfce_corrp_tstat1.nii.gz``. Remember that this particular file shows 1-p values; in other words, if we set the lower bound to 0.95, we will only see those voxels that have a p-value of 0.05 or less. Set the value in the field next to ``Darkest`` to ``0.95``, and you should see a thresholded image that looks like this:

.. figure:: 04_ResultsOverlay.png


Editing the Layout
******************

If you had the crosshairs centered at the peak or the cluster that you wanted, and you removed the crosshairs, you could save this layout as a figure, minus the control bar and surrounding window frame, by selecting ``File -> Save Bitmap``. However, let's stay with this current setup, and see if we can modify the image to be more pleasing and informative to look at.

For example, the default in many viewing software packages is **radiological convention**, in which the right side of the brain is on the left side of the viewer, and vice versa for the left side of the brain. This viewing scheme makes sense if you are looking at the brain from the bottom, which is what many radiologists tend to do. For most people, on the other hand, it makes more intuitive sense to look at the brain in **neurological convention**, in which the left side of the brain is on the left side of the image, and the right side of the brain on the right side of the image. To change this, click on ``MRIcroGL -> Preferences``, and uncheck the box next to ``Radiological convention``. This will flip the brain image and swap the labels.

.. figure:: 04_NeurologicalConvention.png

The current color palette might also be too saturated for your taste. You could either modify the intensity range to sharpen the distinction between voxels with different significance levels, or you could choose another color scheme. In the dropdown menu that says ``1red``, try changing it to ``6warm``. What do you think of this one? Change it again to ``8redyell``. Does this make the figure easier or more difficult to look at and interpret? Ultimately, these choices are up to you; there is no clear right or wrong answer. However, you should start to build up your own standards of taste, and think about why you make certain choices about your illustrations. How would this appear to somebody else who is not familiar with your data? What do you notice about figures you see in other papers that make them clear and easy to understand? These are the questions you can start asking yourself to build up your own aesthetic. Your primary consideration should always be the convenience of the reader; after that, you can make the stylistic choices that make your figures and your prose immediately recognizable, even when unsigned.

