.. _MRtrix_06_TissueBoundary:

==========================================
Chapter #6: Creating the Tissue Boundaries
==========================================

--------------

We are almost ready to begin our streamline analysis, in which we will place **seeds** at random locations along the boundary between the grey matter and the white matter. A streamline will grow from each seed and trace a path from that seed region until it terminates in another region. Some of the streamlines will terminate in places that don't make sense - for example, a streamline may terminate at the border of the ventricles. We will cull these "error" streamlines, and be left with a majority of streamlines that appear to connect distant grey matter regions.

To do this, we will first need to create a **boundary** between the grey matter and the white matter. The MRtrix command ``5ttgen`` will use FSL's FAST, along with other commands, to segment the anatomical image into five tissue types:

1. Grey Matter;
2. Subcortical Grey Matter (such as the amygdala and basal ganglia);
3. White Matter;
4. Cerebrospinal Fluid; and
5. Pathological Tissue.

Once we have segmented the brain into those tissue classes, we can then use the boundary as a mask to restrict where we will place our seeds.

Converting the Anatomical Image
*******************************

The anatomical image first needs to be converted to MRtrix format. Just as we did in a previous chapter, we will use the command ``mrconvert`` (make sure you are in your anatomical directory first):

::

  mrconvert sub-CON01_ses-preop_anat_sub-CON01_ses-preop_T1w.nii.gz T1.mif
  
This creates a new file, ``T1.mif``, which you can look at in mrview.

We will now use the command ``5ttgen`` to segment the anatomical image into the tissue types listed above:

::

  5ttgen fsl T1.mif 5tt_nocoreg.mif

If the segmentation has finished successfully, you should see the following image in mrview:

(insert code here)

.. note::

  If the segmentation step fails, this may be due to insufficient contrast between the tissue types; for example, some anatomical images are either very dark across both the grey and white matter, or very light across both tissue types. We can help the segmentation process by increasing the intensity contrast (also known as **intensity normalization**) between the tissues with a command like AFNI's 3dUnifize, e.g.:
  
  ::
   
    3dUnifize -input anat.nii -prefix anat_unifize.nii
    
  The difference between the image before and after may be subtle, but it can prevent a segmentation error from being thrown.
