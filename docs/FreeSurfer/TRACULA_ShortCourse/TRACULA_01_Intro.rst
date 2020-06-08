.. _TRACULA_01_Intro:

=============================
TRACULA Tutorial #1: Overview
=============================

-------

TRACULA (TRActs Constrained by UnderLying Anatomy) is a suite of diffusion analysis programs included in the FreeSurfer software package. Unlike other packages, such as TBSS or MRtrix, TRACULA focuses on reconstructing the major white matter pathways of the brain, calculating values such as fractional anisotropy and mean diffusivity for each tract. These values can then be compared across groups to see where there are differences between groups, or how these values are correlated with other covariates.

.. note::

  If you try running TRACULA, you may get the following warning:
  
  ::
  
    dyld: lazy symbol binding failed: Symbol not found: ___emutls_get_address
    
  This can cause the rest of the TRACULA pipeline to exit with errors, as the paths will be mis-specified. To fix this, follow the steps outline on `this website <https://www.imore.com/how-turn-system-integrity-protection-macos>`__ to disable your csrutil.
