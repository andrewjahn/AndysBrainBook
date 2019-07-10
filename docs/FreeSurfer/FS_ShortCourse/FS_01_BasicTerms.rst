.. _FS_01_BasicTerms:

===============
= Basic Terms =
===============

Overview
*********

FreeSurfer is particularly attractive because, no matter what other kinds of data you collect during your scan, you will most likely acquire a T1-weighted anatomical image. And unlike a functional scan that involves an experiment with actual planning and hard work, collecting a T1 image is nearly impossible to screw up. And for a structural analysis, a T1 scan is usually all you need.

Here’s what’s different: Instead of analyzing the brain as a 3D volume, FreeSurfer transforms the cortex into a 2D surface. Why a 2D surface? Picture a voxel that straddles both edges of a sulcus. The voxel contains a mixture of signals from both regions, and it is not possible to determine which region contributed to the signal - a problem known as the partial voluming effect.

.. figure:: PartialVoluming.png

  An illustration of the partial voluming effect. A voxel, outlined in red, contains signal from both region A (green) and region B (yellow). The partial voluming effect can also occur when the signal contains both grey matter and white matter.
  

We run into a similar problem when we have a voxel that contains two or more different tissue types. Imagine we have a voxel that contains grey matter, white matter, and cerebrospinal fluid (CSF). In this case we cannot tell how much of each is contained by that voxel: It is a single number which represents each of the different tissue types within the voxel, but it is impossible to tell how much of each tissue type is within the voxel.

.. figure:: PartialVoluming_TissueTypes.png

  The partial voluming effect in a structural scan. The box highlighted in red represents a voxel that encompasses three different tissue types: White matter, grey matter, and CSF. If you imagine that the greyscale image is a real brain, and our red box is the smallest resolution element of our scan, the red box would be an average of the different tissue types contained within it.
  
  
FreeSurfer's Solution
*********

FreeSurfer gets around this problem by tracing the boundaries between the different tissues of the brain - grey and white matter, grey and pial matter, and so on - and then inflating those surfaces into spheres. Most of the leftover defects in the inflated surface are automatically corrected (although some do need to be fixed manually). These surfaces can then be rendered as partially inflated, fully inflated, or spherical brains.

To help you better understand what FreeSurfer does, picture this: You’ve just removed someone’s brain and placed it on the table. The brain is like a flaccid balloon, with the wrinkles representing the gyri and sulci of the cortex. Now, you put your mouth on the severed brainstem (after washing it with soap and hot water, of course) and blow as hard as you can, inflating the brain to its maximum extent. The wrinkles disappear and the brain becomes a fully inflated balloon, like a sphere. This is a different way to view your data - instead of using voxels as the building blocks of our image, we use vertices and edges. Think of this as like a chain link fence wrapped around the surface of the cortex: The intersections of the links are the vertices and the links are the edges. The vertex is now our smallest resolution element, and at each vertex we can calculate structural measurements such as thickness, volume, and surface area.

.. figure:: Recon_Example.png

  Sample illustration of the FreeSurfer reconstruction (recon) process. (A) The T1-weighted anatomical scan is created by the scanner, usually with a resolution of about 1mm cubed. (B) The 3D anatomical image is converted by FreeSurfer’s recon-all into a 2D mesh. The pial surface is displayed here. (C) A closeup of the mesh surface, showing its composition of Vertices (intersections of the triangles making up the mesh) and Edges (connections between vertices).


Once you’ve reconstructed the surface you can resample your statistical maps and view them on an inflated surface (Figure 4), or deflate the surface and see where the activation lies on the original, wrinkled cortex. This gives you a better picture of how the statistical maps lie along the ridges and valleys of the brain.

FreeSurfer uses the reconstructed surface, along with prior knowledge about the topology of a typical human brain, to label the cortical and subcortical structures. The labeling of the cortex is called Parcellation, and the labeling of the subcortical structures is called Segmentation. These labelings are based on the two atlases that come with FreeSurfer: The Desikan-Killiany atlas and the Destrieux atlas, with the Destrieux atlas containing finer-grained parcellations. Structural measurements are then averaged within each parcellation. These measures can then be compared across groups, or correlated with some individual difference measure, such as age, IQ, and satisfaction with your current data relationship.

.. figure:: StatisticalMap_Surface.png

  Brain activity mapped onto the surface. The inflated pial surface is displayed here. Green: Gyri; Red: Sulci. The thresholded activation map is displayed in blue. Note that this type of rendering gives the viewer a better idea of where activity lies within the sulci, is otherwise hidden in a volumetric, 3D view.
  


Video
*******

For a video overview of FreeSurfer and a definition of it's basic terms, see `this video <https://www.youtube.com/watch?v=6wxJ1up-E7E>`__.
