.. _AppendixC_Annotations:

======================================================
Appendix C: Annotating Anatomical Images with Freeview
======================================================

---------------

Overview
********

Freeview, in addition to allowing you to view images and edit them for recon-all, also enables you to **annotate** images. For example, if you want to draw on a T1-weighted anatomical image yourself to trace out where a specific region of the brain is - say, the putamen - you can do so within the Freeview interface. When we select the voxels that we decide are part of a specific region of the brain, we fill those voxels with a certain number - say, the number one - and leave the rest of the voxels as zeros. This is called creating a **mask**, also sometimes referred to as a **Region of Interest (ROI)**.


Annotating a Region of Interest (ROI)
*************************************

First, open Freeview by either double-clicking on the icon in your FreeSurfer directory, or by opening a Terminal and typing ``freeview``. When it opens, click on ``File -> Load Volume`` and select the anatomical image that you want to annotate. When it loads, you should see something like this:

.. figure:: AppendixC_FreeviewLayout.png

In order to create our region of interest, click on ``File -> New ROI``. You will be prompted to type a label for the ROI; in this case, we are annotating the right putamen, so let's call it ``Right Putamen``. Make sure that the dropdown menu below it is the T1 image you are annotating; this will ensure that the ROI has the same resolution and dimensions as the image you are drawing on, also called the **Template Image**. Click OK.

There will now be a small floating window called ``ROI Edit`` that is open as long as you are annotating the ROI. The brush size refers to the size of the radius of the brush, in voxels, which for most anatomical images will be the same as millimeters. For smaller regions and thinner bands of cortex, you should probably use a smaller brush size. In our current example, we will change the brush size to ``3``. Also note that in this window there are other drawing options, such as ``Polyline``, ``Livewire``, and ``Fill``. We will cover these later; for now, leave it as the default of ``Freehand``. 

.. figure:: AppendixC_ROIEdit.png

.. note::

The icons on the top row of the Freeview GUI indicate which viewing or drawing mode is now active; right now you should see highlighted the icon with a pencil on a piece of paper.


Drawing the Region
&&&&&&&&&&&&&&&&&&

Once the drawing tool has been selected, you can left-click on the voxels in any of the viewing panes to begin annotating voxels. You may prefer one viewing pane to another; if you want to only see the Axial pane, for example, click on the icon at the top of the Freeview window that is an Axial view, and then click the white square, which will display only that orthogonal view. You may also choose to change the color and opacity of your annotation by using those options in the left panel of the Freeview GUI:

.. figure:: AppendixC_AxialView.png

.. note:: You can zoom in and out by using your mouse's scrolling function. You can also move between adjacent slices by pressing the up and down arrows on your keyboard.

Now, begin left-clicking and dragging to draw your annotation on the voxels that you believe belong to the Putamen. Move on to each adjacent slice until you are able to annotate the entire region.

.. figure:: AppendixC_Annotation.png

When you are finished drawing your ROI, you can save it by clicking ``File -> Save ROI As``. This will automatically save it as a ``.label`` file, which we can later convert into NIFTI format using the command ``mri_label2vol``.

You can annotate many different ROIs simultaneously. For example, if you select ``File -> New ROI`` and name the new ROI ``Left Putamen``, make sure it is highlighted in the left panel of Freeview. Then, you can choose a different color for it, and annotate it as you like. Whichever ROI is highlighted will be written to disk when you select ``File -> Save ROI As``.

.. figure:: AppendixC_TwoROIs.png

.. note::

  If you make a mistake during annotation, you can press ``command + z`` to undo the last stroke. Typing ``Shift + command + z`` will redo the last stroke. Holding down shift and left-clicking will remove any annotated voxels from the ROI that is currently highlighted in the selection pane.

Saving the ROIs as NIFTI
************************

Label files are able to be read only by FreeSurfer, but you may wish to use or view the ROIs in other software as well. To make them as portable as possible between groups, you can conver the label file to a NIFTI file using ``mri_label2vol``. For example, this line of code, executed from the directoy containing both the T1.mgz file and the RightPutamen.label file, will convert RightPutamen.label into RightPutamen.nii.gz:

::

  mri_label2vol --label RightPutamen.label --temp T1.mgz --regheader T1.mgz --tkr-template T1.mgz --o RightPutamen.nii.gz
