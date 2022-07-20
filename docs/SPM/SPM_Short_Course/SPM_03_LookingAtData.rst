.. _SPM_03_LookingAtData:

====================================
SPM Tutorial #3: Looking at the Data
====================================

----------------

Overview: The SPM Graphical User Interface
---------

Now that you've downloaded the dataset, you will want to **look at your data** - for example, you will want to know if there are any artifacts or problems with your data, and whether these can be alleviated by :ref:`preprocessing <SPM_04_Preprocessing>`. 

Let's first rename the dataset to something that is clear and informative. If the dataset has been downloaded to your Downloads directory, use the Matlab terminal to navigate to the Desktop and type the following:

::

    movefile('~/Downloads/ds000102_0001/', 'Flanker')
    
Which will rename the folder to ``Flanker`` and put it on your Desktop.


    
    
As you saw in the previous :ref:`Data Download page <SPM_01_DataDownload>`, the dataset has a standardized structure: Each subject folder contains an anatomical directory and a functional directory labeled ``anat`` and ``func``, and these in turn contain the anatomical and functional images collected during the experiment. (The ``func`` directory also contains **onset times**, or timestamps for when the subject underwent either a Congruent or Incongruent trial.) This format is known as `BIDS <http://bids.neuroimaging.io/>`__, or Brain Imaging Data Structure, which makes it easy to organize and analyze your data.


.. figure:: 03_Flanker_DataStructure.png

    Example of the BIDS format. Note that the ``func`` directory contains functional data - in this case, two runs of functional data - and corresponding "events.tsv" files, which contain **onsets**, or timestamps of which condition happened at what time. You can open these as a text file or as a spreadsheet. We will use these later when we build our General Linear Model.
    
To look at and inspect the data, we will be using the **SPM Graphical User Interface**, or GUI for short. You can open the GUI by opening a new Matlab terminal, typing ``spm fmri`` from the command line, and pressing enter.



--------

Inspecting the Anatomical Image
----------
    
Whenever you download imaging data, check the anatomical and functional images for any **artifacts** - scanner spikes, incorrect orientation, poor contrast, and so on. It will take some time to develop an eye for what these problems look like, but with practice it will become quicker and easier to do.

To begin, let's take a look at the anatomical image in the ``anat`` folder for ``sub-08``. If you haven't already opened SPM, navigate to the sub-08 folder and then type

::

    spm fmri
    
and press return, which will open the SPM graphical user interface. If you click on the ``Display`` button, you will be prompted to select an image. 

.. note::

  SPM can read any image that are in NIFTI format, but they cannot be compressed - that is, if the datasets end with a ``.gz`` extension, you will first need to unzip them by navigating to the directory containing the images and then type

  ::

    gunzip('*.gz')
    
  Which will expand the images and remove the ``.gz`` extension.


.. figure:: 03_Anatomical_Inspection.png

    The anatomical image displayed in the SPM viewer in axial, sagittal, and coronal views. You can close any of the windows if you only want to focus on a subset of the views. 
    
   
Inspect the image by clicking around in one of the viewing windows. Notice how the other viewing windows and crosshairs change as a result - this is because MRI data is collected as a three-dimensional image, and moving along one of the dimensions will change the other windows as well.

.. note::

    You may have noticed that this subject appears to be missing their face. That is because the data from OpenNeuro.org have been **de-identified**: Not only has information such as name and date of scanning been removed from the header, but the faces have also been erased. This is done in order to ensure the subject's anonymity.
    

As you continue to inspect the image, here are two things to watch out for:

1. Lines that look like ripples in a pond. These are called **Gibbs Ringing Artifacts**, and they may indicate an error in the reconstruction of the MR signal from the scanner. These ripples may also be caused by the subject moving too much during the scan. In either case, if the ripples are large enough, they may cause preprocessing steps like brain extraction or normalization to fail.

.. figure:: 03_Gibbs.png

    Photo credit: Sundar Amartur


2. Abnormal intensity differences within the grey or the white matter. These may indicate pathologies such as aneurysms or cavernomas, and they should be reported to your radiologist right away; make sure you are familiar with your laboratory's protocols for reporting artifacts. For a gallery of pathologies you may see in an MRI image, click `here <http://www.mrishark.com/brain1.html>`__.

----------

Inspecting the Functional Images
----------
    
When you are done looking at the anatomical image, click on the ``Display`` button again, navigate to the ``func`` directory, and select the ``run-1`` functional image.

A new image will be displayed in the orthogonal viewing windows. This image also looks like a brain, but it is not as clearly defined as the anatomical image. This is because the **resolution** is lower. It is typical for a study to collect a high-resolution T1-weighted (i.e., anatomical) image and lower-resolution functional images, which are lower resolution in part because they are collected at a very fast rate. One of the trade-offs in imaging research is between spatial resolution and temporal resolution: Images collected at higher temporal resolution will have lower spatial resolution, and vice versa.

.. figure:: 03_Functional_Inspection.png


Many of the quality checks for the functional image are the same as with the anatomical image: Watch out for extremely bright or extremely dark spots in the grey or white matter, as well as for image distortions such as abnormal stretching or warping. One place where it is common to see a little bit of distortion is in the orbitofrontal part of the brain, just above the eyeballs. There are ways to `reduce this distortion <https://andysbrainbook.readthedocs.io/en/latest/FrequentlyAskedQuestions/FrequentlyAskedQuestions.html#how-can-i-unwarp-my-data>`__, but for now we will ignore it.

.. Reference the time-series glossary

Another quality check is to make sure there isn't excessive motion. Functional images are often collected as a time-series; that is, multiple volumes are concatenated together into a single dataset. To view the time-series of volumes in rapid succession, click the ``Check Reg`` button and load the ``sub-01_task-flanker_run-1_bold.nii`` data. This will display a single volume in three planes: Coronal, Sagittal, and Axial. Right click on any of the planes and click the ``Browse`` button. You will be prompted to select an image; click on the currently selected file to remove it, and then enter the string ``run-1`` in the Filter field, and ``1:146`` in the Frames field. Select all of the resulting images, and click ``Done``.

You will now see a horizontal scrolling bar at the bottom of the display window. Clicking on the right or left arrows will advance or go back one volume; you can also click and drag the scrolling bar to view the volumes more rapidly. Clicking on the ``>`` button in the bottom right will start **movie mode**, which flips through the volumes at a rapid pace. Clicking on the button again will stop the movie. To see a plot of the time-series activation at the voxel under the crosshairs, right-click again on any of the planes, select "Browse", and then select "Display profile". This opens up another figure that you can view simultaneously as you flip through the volumes.

.. figure:: 03_SPM_ViewTimeSeries.gif

Also, during the :ref:`Realignment preprocessing step <01_SPM_Realign_Unwarp>` you will generate a movement parameter file showing how much motion there was between each volume. To begin learning about the preprocessing steps, click the `Next` button.


--------

Exercises
-----------

1. View the time-series of the ``run-2`` data, using the steps outlined above. Do you notice any sudden changes in movement? View the time-series for ``run-1``, and compare it to ``run-2``. Which volumes, if any, show any sudden changes in movement?

2. Examine a few of the other anatomical and functional scans for some of the other subjects, making sure to unzip the images before loading them into the viewer. How does the contrast and the brightness change as you drag the crosshair through different slices of the image? What do you think affects the brightness of a given slice?

3. If you are viewing one of the functional images using the ``Display`` button, right-clicking on any of the viewing panes will display a menu with the currently viewed file name at the top of it. Hover your mouse over the file name, and observe the values that are presented in a sub-menu on the right. How do these compare with the values that you see in the bottom half of the Display window?

4. SPM reads **header information** when it loads a file. The command line version of this is called ``spm_vol``. From the Matlab terminal, navigate to the directory ``sub-01/func``, and type the following:

::

    run1 = spm_vol('sub-01_task-flanker_run-1_bold.nii')
    
Notice that there are several fields that are returned in this structure, such as fname, dim, and dt. You can examine the contents of each one of them by typing, e.g.,

::

    run1.fname
    
In this case, why are there 146 answers that are returned? Which of the fields contains the dimensions of the voxels for each volume? Which of the fields contains the dimensions of the overall volume (i.e., width, length, and height)? How many volumes would be returned if you applied the ``spm_vol`` command to the anatomical image? Why?

5. Open the anatomical image for sub-08 in the Display Image viewer, and right click on any of the three window panes. Select ``Overlay -> Add Image -> This Image``, and select the functional file ``sub-08_task-flanker_run-1_bold.nii``. The functional image will be overlaid on the anatomical image and displayed in a red-orange heatmap, showing a relatively good initial alignment between the images:

.. figure:: 03_ImageOverlay.png

Now do the same procedure for the anatomical and functional images for sub-01, which should give you a figure like the following:

.. figure:: 03_ImageOverlay_sub01.png

What do you notice? This misalignment between the images will be addressed in a later chapter on :ref:`Setting the origin <SPM_07_SettingTheOrigin>`.

Video
--------

For a video overview of how to check the quality of your data, click `here <https://www.youtube.com/watch?v=j0AEAOghD7w>`__.
