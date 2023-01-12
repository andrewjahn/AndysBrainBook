.. _ITK-Snap_02_GUI:

================
The ITK-Snap GUI
================

---------------

ITK-Snap is run primarily from the Graphical User Interface. Once you load an image, you are able to make edits to the image, labeling voxels with different values as you see fit.

Let us continue with our example from the Infant Brain Imaging Study (IBIS) dataset, which contains anatomical images of children from birth to 24 months of age. When you open ITK-Snap, it will prompt you to select a recently opened image, if you have previously used the software. If it is your first time using ITK-Snap, click on ``File -> Open Main Image`` (or, alternatively, the ``Open Image`` button in the bottom right corner of the GUI), and select the skull-stripped, T1-weighted anatomical image that you have analyzed with infant recon-all. Using our current example subject, ``IBIS0079``, select the file ``brainmask.mgz`` located within the folder ``IBIS0079/mri``:

.. figure:: 02_LoadImage.png

After you have loaded the image, you should see three orthogonal views of the brain, along with a three-dimensional reconstruction in the bottom left panel, if applicable (it does not exist in our current example):

.. figure:: 02_OrthogonalViews.png

Now that we have loaded the brainmask, we will overlay the segmentation to see how well the grey and white matter tissues are outlined. Click on ``Segmentation -> Add Segmentation``, and click the ``Browse`` button from the resulting pop-up window. Then navigate to the directory ``IBIS0079/mri``, and select the file ``aseg.nii.gz``, and click ``Open``. Click ``Next`` and ``Finish`` on the resulting pop-up windows:

.. figure:: 02_AddSegmentation.png

This will overlay the segmentation on top of the brainmask image, which should now look like this:

.. figure:: 02_SegmentationOverlay.png
