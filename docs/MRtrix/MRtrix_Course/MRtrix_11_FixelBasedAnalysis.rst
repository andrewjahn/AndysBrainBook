.. _MRtrix_11_FixelBasedAnalysis:


=========================================
MRtrix Tutorial #11: Fixel-Based Analysis
=========================================

--------------

Overview
********

Until now, we have focused on generating **streamlines**, representations of the underlying white-matter tracts. These are probabilistic tracts, which take into account nearby tissue types and amount of angle in order to create biologically plausible connections between distant cortical regions. MRtrix also allows for the generation of **fixels**, or specific fiber populations within a voxel.

Remember that traditional diffusion-tensor methods cannot resolve the crossing-fibers problem, in which two bundles of fibers crossing at right angles will give the appearance of uniform diffusion within that voxel. By using constrained spherical deconvolution (CSD) with a single fiber orientation as a basis function, we can resolve the signal measured within a voxel into its constituent fibers, or fiber orientation distributions (FODs). This can then be used to quantify the fiber density (FD) of the underlying white matter, as well as changes to the overall size of the cross-section within a given voxel (fiber cross-section, or FC). These two metrics can furthermore be combined into a single metric called fiber density and cross-section (FDC).

`Raffelt et al., 2015 <https://www.sciencedirect.com/science/article/pii/S1053811916304943#f0015>`__, illustrated the difference in these metrics by using an example 7x7 matrix of voxels. Given a certain amount of white matter fibers within the voxels, a reduction in the number of fibers can happen in three different ways:

1. The amount of white matter fibers occupies the same number of voxels, but the overall density decerases (reduced FD); or
2. The density stays the same, buy the white matter fibes occupy fewer voxels (reduced FC); or
3. Both the number of occupied voxels and the density of white matter within those voxels decrease (reduced FDC).

The opposite of each of these scenarios could also take place, which would lead to increases in each of those metrics, respectively.

.. figure:: 11_Raffelt_2017_Fig1.jpg


Preparing the Data for Fixel-Based Analysis
*******************************************

Some of the steps for doing a Fixel-Based Analysis are similar to what was done in the previous chapters of this module. There are some significant differences, however, once the Fiber Orientations are estimated and then warped to a template.

If you are analyzing a large number of subjects - for example, several dozen or more - you will either need a computer with at least a couple of hundred gigabytes of memory, and ideally multiple processors. Fixel-based analysis can be done on a local machine, but it can take a long time; several days, depending on how many subjects you have. Regardless, you can process these images by using MRtrix's ``for_each`` command (details about the command can be found `here <https://mrtrix.readthedocs.io/en/dev/reference/commands/for_each.html>`__). To illustrate how to use this batch command, we will take a relatively simple case of denoising the raw diffusion-weighted data. 
