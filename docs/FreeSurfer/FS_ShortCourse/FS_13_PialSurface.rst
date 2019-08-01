.. _FS_13_PialSurface:


======================================
FreeSurfer Tutorial #13: SkullStripping and Pial Surface Errors
======================================

---------------

SkullStripping
*********

One of the first preprocessing steps in recon-all is to :ref:`**skullstrip** <Skull_Stripping>` the anatomical image. This removes both the skull and anything else from the image that is not grey or white matter, such as the eyes, neck, ears, and dura mater, which allows recon-all to trace a more accurate boundary of the pial surface.

.. figure:: 13_SkullStripping_BeforeAfter.png

  The T1-weighted anatomical image before and after skullstripping. On the right, the outline of the pial surface is traced in red.

The pial surface after skullstripping should be checked for whether cortex was removed during skullstripping, or not enough non-brain tissue was removed. In the latter case, the pial surface may include part of the dura matter or the skull, which can inflate the estimate of the amount of cortex in that region.

.. note::

  Even if not all of the non-brain material was removed during skullstripping, that is OK as long as the pial surface has been correctly traced.
  
Setting the watershed threshold
^^^^^^^^^^

One way to control the amount of skull that is removed is by 


Pial Surface Errors
**********


---------


Video
*********
