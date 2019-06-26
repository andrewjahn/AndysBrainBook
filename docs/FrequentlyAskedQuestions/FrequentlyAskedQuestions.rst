.. _FrequentlyAskedQuestions:

Frequently Asked Questions
==============

This is a list of common questions that I am asked. I have found that most questions can be organized into categories such as Resampling, Cluster Correction, Normalization, and so on. Some of these questions may eventually be folded into the fMRI Concepts section.


Resampling
**********

Question: What is resampling? 

Answer: Resampling means changing either the resolution, dimensions, or both the resolution and dimensions of an image. A typical fMRI image is composed of voxels, which are like the pixels that make up your computer screen, but in three dimensions. For example, a common voxel size is 3 by 3 by 4 millimeters. 

.. figure:: Voxel_Example.png

  An fMRI image is composed of cubes called voxels (lower right). Each of these cubes contains a single number representing the signal measured at that voxel. Voxels can either be isotropic, with dimensions of equal lengths, or anisotropic, with at least one dimension either longer or shorter than the other dimensions. One common use of resampling is to make the dimensions of the voxel either shorter or longer, which in turn creates larger or smaller voxels. Larger voxels lead to a lower-resolution image, and smaller voxels will lead to a higher-resolution image.
  

You can resample an image using several different methods. Let's start with the nearest neighbor method, which is the easiest to illustrate. Imagine that we have a 4x4 grid with a number in each square, and that we want to resample it to a 3x3 grid. In the animation below, we superimpose a 3x3 grid on the original 4x4 grid and then find which number is closest to the center of the new square. In the upper left corner, for example, the number 8 is closest to the center of the new square. As a result, the new square will now contain the value 8.

.. figure:: NearestNeighbor_Example.gif


Biased Analysis
**********

Question: What is a biased analysis? How do you avoid doing them?

To give an example of a biased analysis, imagine that we wanted to test whether drinking Four Loko helps undergraduates do better on their exams. Let’s say that we observed which students showed an improvement, and then ran our final group-level analysis on only those students. Obviously this would be a biased analysis, since we’re only focusing on those subjects that have the effect we’re looking for; it’s no longer a truly random sample.

Circular analyses can also happen with imaging data, although it’s not as apparent when it happens. This was first pointed out in a study which examined activity in the fusiform face area in response to different stimuli. They extracted data from each condition’s significant voxels and discovered a pattern of selective activity. However, it was pointed out that if you chose an ROI outside of the brain which happened to contain significant voxels just by chance, and ran the same tests on those voxels, you would get the same pattern - which clearly shouldn’t happen. When they reran the analysis using independent ROIs, they found a pattern of noise - which you would expect with a non-brain ROI. When they ran an unbiased ROI analysis on the original data, they found that the original pattern disappeared.

Let’s see exactly how biased ROIs lead to inflated effect sizes. Here is a z-score map showing our group-level effects. If we zoom in, we can see the boundaries of each individual voxel, with a range of z-scores from 0 to 3. Assume that there is a real effect in the brain outlined in orange. If we extracted the parameter estimates for each subject from those voxels, the effect would be 0.3 – with some variation around that value. Now assume that we threshold our image at an uncorrected level of p<0.05, or a z-score of 1.65. The voxels highlighted in red are the only ones that pass that threshold. Here’s the important part: notice that this region overlaps with some of the true effect voxels, but that it includes some noise voxels as well. Because the region by definition can only include voxels passing a certain threshold, it will only contain noise voxels that are above that threshold, which biases the effect to be larger than the true effect. If we used an independent ROI, for example with cross-validation, we would create a region that probably contains some true effect voxels, and also some noise voxels – but these noise voxels will not be biased to be artificially high or low. In this example the unbiased effect is slightly lower than the true effect, but in theory it could be higher or lower – it just won’t be biased either way.

.. figure:: BiasedAnalysis.png

There are two problems with those arguments. First, the magnitude of the effect is just as important as detecting whether the effect is there, and biased analyses will systematically overestimate it. Why? Because small studies by definition can only detect large effects. The second is that if you publish a biased analysis, the reader may assume that it is an inferential analysis, even if it includes caveats about how it was done. If you absolutely insist on presenting them in a figure, at least don’t include error bars.

We’ve only touched on a couple of different ways to do biased analyses, but there are other ways too - and you need to be on the lookout for them. Let’s say that you use an anterior cingulate cortex ROI for your confirmatory analysis - meaning that you selected the ROI beforehand, regardless of what the whole-brain results look like - but the results don’t pass correction. You then look at the whole-brain map, and see this. You then decide to use an ROI located more in the pre-SMA. This is also a biased analysis, because now you know where your effect is before you decide where to extract from.


Orientation Problems
**********

Question: When I open my image in a viewer, the axes don't look right. How can I change it to a more reasonable orientation, such as LPI?

First, let's define the acronyms often used when discussing orientation. Remember that fMRI data is three-dimensional, and that each image has an **origin** which specifies the coordinates of X=0, Y=0, and Z=0. Usually the **anterior commissure**, a bundle of connective fibers just below the fornix, is set as the origin.

.. figure:: AnteriorCommissure.png
  :scale: 10%

The orientation of the image indicates which direction relative to the origin is positive or negative, and the orientation is specified by a triplet of letters. For example, LPI signifies that the direction is negative to the left of the anterior commissure, and positive to the right; negative behind, and positive forward; and negative below, positive above. In this orientation, coordinates of X=-3, Y=18, Z=34 would mean that the crosshair is centered on a voxel that is, relative to the anterior commisure, 3 millimeters to the left, 18 millimeters forward, and 34 millimeters above - approximately in the left dorsal anterior cingulate.

Sometimes the orientations are flipped along one or more of the axes, resulting in orientations such as RPI or RAI. As long as all of the data is processed the same way and all of the images have the same orientation, this usually isn't a problem. However, if you have an image with a different orientation, you will have to change it.

This can be done with FSL's fslswapdim command. Let's demonstrate this with the `EUPD Cyberball <https://openneuro.org/datasets/ds000214/versions/00001>`__ dataset from Openneuro.org. If you download the anatomical and functional data for subject EESS001, you will notice that although the functional data looks OK, the anatomical data's orientations appear to be flipped: The coronal section is displayed as though it's on its side, and the other views look odd:

.. figure:: anat_flipped.png
  :scale: 20 %

To fix this, type the following command:

fslswapdim sub-EESS001_anat_sub-EESS001_T1w.nii.gz RL PA IS anat_reorient.nii

When you open the reoriented image, it looks as though it's in the correct orientation. Overlay the functional image on top of it to make sure that all of the images are now in the same orientation.

.. figure:: anat_reorient.png
  :scale: 20 %


What is Signal-to-Noise Ratio? How can I calculate it?
****************


How can I calculate the number of voxels in a mask?
****************

Let's say you have two masks in an image, labeled A and B. Mask A is composed of 1's, and Mask B is composed of 2's. If these masks are saved into one image called ``ROIs.nii.gz``, and they were created from a template called ``ROI_Template.nii.gz``, you can use the command:

::

  fslstats -K ROIs.nii.gz ROI_Template.nii.gz -V

Which will return two numbers per mask. The first number is the number of voxels, and the second number is the volume, in cubic millimeters. For example, if one of my masks was 9 voxels large and the other one was 15 voxels, with a 2x2x2mm resolution (or 8 cubic millimeters per voxel), the output would look like this:

::

  9 72.000000 15 120.000000
  
  
How can I unwarp my data?
****************

note::

  I will expand upon this in a more developed section; the following are some quick notes, so that I don't forget how I did this.

Imaging data is often warped because of magnetic field inhomogeneities (also known as B0 inhomogeneities). The data can be unwarped using field maps, which detect where the inhomogeneities are located.

Another way to unwarp the data is with **blip-up/blip-down** images. Usually these are acquired in the Anterior-to-Posterior (AP) and Posterior-to-Anterior (PA) directions, with one of the directions being used to acquire your functional runs. For example, let's say that you have two images labeled AP.nii.gz and PA.nii.gz: The former contains three volumes, and the latter contains three volumes. AP images typically look more "smushed" near the frontal pole, and PA images are more smeared outwards at the frontal areas.

.. Insert figures of AP and PA examples

You can use FSL's topup to fix these. (Apply motion correction before or after?) First, merge the two phase-encoded images together with ``fslmerge -t AP_PA_b0.nii.gz AP.nii.gz PA.nii.gz``.

Then use topup to create a fieldmap:

::

  topup --imain=AP_PA_b0.nii.gz --datain=acqparams.txt --config=b02b0.cnf --out=topup_AP_PA_b0
  
In which config is a file that is provided by default by FSL (e.g., you don't have to create it; you can type this command from anywhere), and acqparams is a text file that contains the following:

0 -1 0 0.0665
0 -1 0 0.0665
0 -1 0 0.0665
0 1 0 0.0665
0 1 0 0.0665
0 1 0 0.0665


The way to read this file is, in columns from left to right:

1. +RL
2. +PA
3. +IS (This is a guess)
4. Readout time, defined as the time from acquisition of the center of the first echo to the center of the last. You can also calculate it with the formula: ReadoutTime = [EchoSpacing (in ms)] * [EPI Factor] * 0.001

This will createa a field map, which can be applied to the fMRI data with:

::

  applytopup --imain=fMRI.nii.gz --topup=topup_AP_PA_b0 --datain=acqparams.txt --inindex=1 --out=fMRI_unwarped --method=jac

Other Questions
**********

1. What is the difference between a functional and a structural image?
2. Where do the fMRI templates come from? When should one use a template other than the default?
3. What are the types of images that one can generate from the scanner, and how are they different? What questions can they answer?
  
