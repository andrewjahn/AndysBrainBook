.. _04_fASL_GUI:

=========================
fASL Tutorial #4: The GUI
=========================

----------

Overview
**********

Navigate to the directory that contains ``run_1`` and ``run_2``. When you open the fASL GUI by typing ``fasl02`` from the command line in Matlab, you will see this window:

.. figure::

  04_fASL_GUI_Default.png
  
The GUI is divided into several windows, each of which deal with a different aspect of ASL processing. Certain steps are greyed out, and only become useable when a box is checked - for example, you won't be able to create a design matrix unless you check the box next to ``GLM estimation and stats``. We will walk through what each step does, and which options you may want to change in order to adapt them to your analysis.


.. note::

  Not all of the steps are needed to analyze the dataset. For the purposes of this tutorial, we will omit the ``Quantify CBF`` and ``Reconstruction`` steps.


Input Data File (Required)
^^^^^^^^^^^^

.. figure:: 04_InputDataFile.png

Click the button ``Input Data File`` to open a window that you can use to select your input data. The ASL data has already been reconstructed for you - that is, it has been converted to a format that can be analyzed with SPM's preprocessing tools. Navigate to the directory ``run_1``, select the file ``vol_e9632_02_21_119_0299.nii``, and then click ``Open``.

.. note::

  fASL is able to reconstruct raw data as well. In that case, you would load your raw data, and then check the box next to ``Stack of Spirals Recon (UM)`` in the ``Reconstruction`` box. Check with your scan technician whether the scan used GRAPPA along the Z-axis; if so, check the corresponding box.
  
  
Constants & Acquisition Params
^^^^^^^^^^^^^^

This window contains the parameters that were used to acquire the scan, such as TR, T1 decay, and arterial transit time. We will only change one of the defaults: Decrease the ``N. of M0 frames`` from 6 to 0. When you are finished, the window should look like this:

.. figure:: 04_ConstantsAcquisitionParams.png


Time Series Preprocessing
^^^^^^^^^^^^^^

These steps are similar to the preprocessing steps described in the :doc:`FSL Short Course <https://andysbrainbook.readthedocs.io/en/latest/fMRI_Short_Course/fMRI_04_Preprocessing.html>`__; review those steps for more details about what each one does.

Since these reconstructed data have already been slice-time corrected, leave that box unchecked. We also do not have physiological regressors for this data set, so we will leave that box unchecked as well. Check the rest of the boxes: Realignment, Smoothing, Subtraction, CompCorr, and Spatial Normalization. After you have checked the box next to Subtraction, you will have two options to choose from: **Pairwise** and **Surround**. These refer to how the contrast images are created from subtracting the control images from the label images. Pairwise subtracts each image from its neighbor, while Surround takes a triplet of images, multiplies the middle one by two, and subtracts it from the surrounding images. In this experiment, we acquired the ASL images with a Surround paradigm; select that option by clicking on it with your mouse.

.. figure:: 04_TimeSeriesPreprocessing.png

  The preprocessing window should have these boxes checked, and the Surround option highlighted.
  
  
Spatial Transformations
^^^^^^^^^^^^^^^

Since we have decided to do Spatial Normalization by checking that box in the Time Series Preprocessing window, we will need to point to the location of the subject's anatomical image, and a template image to normalize the data to. Click on ``Coregister to Structural``, and navigate to the subject's ``anatomy`` directory. Select ``eht1spgr_124sl.nii``, which is a skull-stripped anatomical dataset.

Next, click on ``Normalise to Template``. Any normalized template can be used; in this example, we will use one of SPM's standard templates. Navigate to ``~/spm12/canonical``, and select the image ``avg152_T1.nii``.

.. figure:: 04_SpatialTransformations


FMRI
^^^^^^^^^^^^^^

Now that we have all of the preprocessing steps ready to go, we will create a general linear model to analyze the data once it has been preprocessed. This will require creating a design matrix indicating which condition occurred at which time, and for what duration. We will also specify which conditions we want to contrast against each other - which in this example will be the 4-back compared to the 1-back task. The resulting image will show differences in cerebral blood flow between those conditions.

To begin, click on ``Build Design Matrix``. This will open up another window that says ``ASL Design Matrix Builder``. In the field next to ``TR (sec.)``, enter the number ``4``. In the ``Exp. Duration (sec.) field, enter ``1192``.

.. note::

  If you were to look at the header of the unprocessed functional data, you would notice that there 300 time points. Since the TR is 4 seconds, it follows that the total duration of the run is 1200 seconds. Why then do we type 1192 in this window? When we specified Surround subtraction in the Time Series Preprocessing window, this indicated to fASL to remove the first and last TRs from the time series, as they do not have both left and right neighboring volumes. This change in the duration of the run will also be reflected in the Onsets field below, which we turn to next.
  
  


  
