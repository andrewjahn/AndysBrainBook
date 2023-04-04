.. _MRtrix_11_FixelBasedAnalysis:


=========================================
MRtrix Tutorial #11: Fixel-Based Analysis
=========================================

--------------

Overview
********

Until now, we have focused on generating **streamlines**, representations of the underlying white-matter tracts. These are probabilistic tracts, which take into account nearby tissue types and amount of angle in order to create biologically plausible connections between distant cortical regions. MRtrix also allows for the generation of **fixels**, or specific fiber populations within a voxel.

Remember that traditional diffusion-tensor methods cannot resolve the crossing-fibers problem, in which two bundles of fibers crossing at right angles will give the appearance of uniform diffusion within that voxel. By using constrained spherical deconvolution (CSD) with a single fiber orientation as a basis function, we can resolve the signal measured within a voxel into its constituent fibers, or fiber orientation distributions (FODs). This can then be used to quantify the fiber density (FD) of the underlying white matter, as well as changes to the overall size of the cross-section within a given voxel (fiber cross-section, or FC). These two metrics can furthermore be combined into a single metric called fiber density and cross-section (FDC).

`Raffelt et al., 2015 <https://www.sciencedirect.com/science/article/pii/S1053811916304943#f0015>`__, illustrated the difference in these metrics 
