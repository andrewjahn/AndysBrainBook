.. _CONN_05_Preprocessing:

========================
Chapter #5: Preprocessing
========================

------------------


Overview
********

Preprocessing a resting-state dataset is similar to how you preprocessing a task-related dataset: We have the same preprocessing steps of realignment, normalization, smoothing, and so on. The major difference between analyzing the two types of datasets is the threshold that we allow for motion. Resting-state datasets are notoriously susceptible to motion-related artifacts - even small movements can introduce spurious correlations. These artifacts are particularly problematic for laboratories studying disorders such as schizophrenia, or for researchers who scan very old or very young subjects. 

There is no simple way to address this problem, aside from training the subjects to move as little as possible. That said, CONN uses several methods for mitigating movements artifacts, including ART (Artifact Removal Toolbox) and principal component filtering of signal from the white matter and cerebrospinal fluid. To perform all of the classical preprocessing steps as well as removal of movement artifacts, click on the ``Preprocessing`` button at the bottom left of the screen. 

.. figure:: 05_Preprocessing_Menu.png

This will open a menu showing virtually every preprocessing step available. Bold font indicates a pipeline, or series of preprocessing steps that have been arranged in a particular order. The first option that is highlighted, "default preprocessing pipeline", will do a traditional analysis that normalizes volumetric data to MNI space. If you are a more experienced user, you may want to explore some of the other options, such as using FreeSurfer to analyze the subject's data in their native space. For now, highlight the default preprocessing pipline and click ``OK``.

This will add all of the preprocessing steps into the window at the top of the screen. Highlighting a step will display a short description of what the step does, along with the input and output. Some steps, such as "functional Direct Segmentation & Normalization", allow you to specify whether you want to use the "First functional volume as reference." (These options probably won't affect your results much, if at all, but they are there at your disposal.) You can also move certain steps up or down in the pipelien, or add and remove certain steps, by using the button on the right of the menu. For now, click the ``Start`` button to begin preprocessing the individual subject that we have loaded.

.. figure:: 05_Preprocessing_Pipeline.png


Before the preprocessing can start, you will be prompted to enter a few more options. For example, you need to specify the slice order; ask your scan technician if you are unsure of what order was used. In this tutorial, we will selected the interleaved (Siemens) ordering. You will also be asked to specify the threshold for how ART identifies an outlier. The "intermediate settings (97th percentile)" should work fine for most cases, although you may want to set it to a higher or lower threshold depending on the population you are studying. Instead of basing the threshold on a normative sample, moreover, you can instead choose to edit the settings directly by selecting "Edit settings" and manually setting the subject-motion mm threshold.

.. figure:: 05_Set_Motion_Threshold.png

For our current tutorial, we will leave it at the intermediate settings.

You will next be prompted to select the sampling resolution of the anatomical and functional output. The defaults of 1mm^3 for the anatomical image and 2mm^3 for the functional images should be fine; if you want to take up less space on your hard drive, you can lower the resolution (i.e., increase the numbers in the fields), at the expense of lower spatial resolution.

You will lastly be asked to specify a smoothing kernel. The default is 8mm, but I will set it to 6mm for this tutorial. Click ``OK``, and the preprocessing will begin, calling upon SPM tools as needed. For this subject, it will take about 5 minutes total.

.. note::

  For more details on what each step does, see the :ref:`SPM preprocessing module <SPM_04_Preprocessing>`.

Next Steps
**********

If everything has run without error, you should see a pop-up window saying that everything has finished without any problems. When the preprocessing has finished, we will need to inspect the images for any artifacts or other problems - in other words, we will do **Quality Assurance (QA) checks**. To learn more about how to do them, click the ``Next`` button.
