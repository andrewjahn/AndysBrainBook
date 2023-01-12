.. _ITK-Snap_02_GUI:

======================================
ITK-Snap Tutorial #2: The ITK-Snap GUI
======================================

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

Before you start editing, you can change the contrast settings of the brain so that it’s easier to see through the segmentation. For example, you can turn by either pressing the ``S`` key, or moving the slider in the lower left corner of the GUI to make the segmentation either more or less transparent. Find a level that you are comfortable with; I prefer somewhere around the 30-40 range for the opacity slider, so that the segmentation is visible as well as the underlying structures of the anatomical image.

Another way to improve visualization of the images is to change the contrast between the tissue types. Above the opacity bar and the segmentation labels, right-click on the brain you are currently editing (in our case, ``brainmask``), and then click ``Contrast Inspector``. This will open a new window with two dots above a historgram of intensity values. Click and drag the dots until the contrast of the underlying brainmask is to your liking:

.. figure:: 02_ContrastInspector.png


Appendix: Additional Controls and Commands
******************************************

Below is a list of keyboard shortcuts you may find useful when using ITK-SNAP:

Any mode:
●	Open up a main image: Command-G
●	Open up a segmentation: Command-O
●	Undo: Command-Z
●	Redo: Shift-Command-Z
●	Toggle between options in the main toolbar: number keys 1-6
●	Change opacity: “A” moves the opacity down 5 and “D” moves the opacity up 5. “S” toggles the opacity on and off.
●	Change labels: “<” moves down one label and “>” moves up one label
●	Pan across a plane: Hold down middle click (scroll wheel) and drag
●	Scroll through image slices: Top left panel = scroll up and down with the scroll wheel. Top right panel = left and right arrow keys. Bottom right panel = up and down arrow keys.

Crosshair and zoom mode (1 and 2):
●	Zoom in and out: Hold right click and drag up or down
●	Move the cursor: Hold down left click and drag in cursor mode (1)

Paintbrush mode (4):
●	Add voxels: Hold down left click and drag
●	Remove voxels: Hold down right click and drag
●	Change paintbrush size: “-” cuts down the brush size by one voxel and “+” brings the brush size by one voxel.
