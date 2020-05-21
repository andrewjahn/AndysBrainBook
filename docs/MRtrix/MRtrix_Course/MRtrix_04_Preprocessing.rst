.. _MRtrix_04_Preprocessing:

=================================
MRtrix Tutorial #4: Preprocessing
=================================

---------------

Overview
********

Just like other neuroimaging data, diffusion data should be **preprocessed** before it is analyzed. Preprocessing removes sources of noise from the image, such as movement artifacts and other distortions. Diffusion data in particular is susceptible to **warping artifacts** as a result of the phase-encoding direction: In general, the predominant encoding direction - such as Anterior to Posterior, or AP - will make the anterior part of the brain look more "squished", as though a strong headwind is blowing from the Anterior direction. The opposite is true of the Posterior to Anterior, or PA, phase-encoding direction. Sometimes these distortions are very subtle, but other times they are conspicuous:

.. figure:: 04_AP_PA_Comparisons.png

The following are common preprocessing steps done with MRtrix. If you have used the software package FSL to analyze diffusion data, note that some of the FSL commands - such as eddy and topup - are used in some of the MRtrix libraries. We will explore that more below.


dwi_denoise
***********

The first preprocessing step we will do is **denoise** the data by using MRtrix's ``dwidenoise`` command. This requires an input and an output argument, and you also have the option to output the noise map with the ``-noise`` option. For example:

::

  dwidenoise sub-01_dwi.mif sub-01_den.mif -noise noise.mif
  
This command should take a couple of minutes to run.

One quality check is to see whether the residuals load onto any part of the anatomy. If they do, that may indicate that the brain region is disproportionately affected by some kind of artifact or distortion. To calculate this residual, we will use another MRtrix command called ``mrcalc``:

::

  mrcalc sub-01_dwi.mif sub-01_den.mif -subtract residual.mif
  
You can then inspect the residual map with mrview:

::

  mrview residual.mif
  
.. figure:: 04_residuals.png

It is common to see a grey outline of the brain, as in the figure above. However, everything within the grey matter and white matter should be relatively uniform and blurry; if you see any clear anatomical landmarks, such as individual gyri or sulci, that may indicate that those parts of the brain have been corrupted by noise. (Maybe give some recommendations about what to do if they see this?)

mri_degibbs
***********

An optional preprocessing step is to run ``mri_degibbs``, which removes Gibbs' ringing artifacts from the data. These artifacts look like ripples in a pond, and are most conspicuous in the images that have a b-value of 0. Look at your diffusion data first with ``mrview``, and determine whether there are any Gibbs artifacts; if there are, then you can run ``mrdegibbs`` by specifying both an input file and output file, e.g.:

::

  mrdegibbs sub-01_den.mif sub-01_den_unr.mif
  
As always, inspect the data both before and after with ``mrview`` to determine whether the preprocessing step made the data better, worse, or had no effect.

If you don't see any Gibbs artifacts in your data, then I would recommend omitting this step.


Extracting the Reverse Phase-Encoded Images
*******************************************

Most diffusion datasets are composed of two separate imaging files: One that is acquired with a primary phase-encoding direction, and one that is acquired with a reverse phase-encoding direction. The primary phase-encoding direction is used to acquire the majority of the diffusion images at different b-values. The reverse-phase encoded file, on the other hand, is used to **unwarp** any of the distortions that are present in the primary phase-encoded file.

To understand how this works, imagine that you are using a blow dryer on your hair. Let's say that you have the blow dryer pointed at the back of your head, and it blows your hair forward, onto the front of your face; let's call this the posterior-to-anterior (PA) phase-encoding direction. Right now your hair looks like a mess, and you want to undo the effects of the air blowing from the back of your head to the front of your head. So you point the blow dryer at the front of your face, and it blows your hair back. If you take the average between the two of those blow dryings, your hair should be back in its normal position.

Similarly, we use both phase-encoding directions to create a sort of average between the two. We know that both types of phase-encoding will introduce two separate and opposite distortions into the data, but we can use unwarping to cancel them out.

Our first step is to extract the first volume of the reverse phase-encoded NIFTI file into .mif format (since the first volume has a b-value of zero):

::

  mrconvert sub-01_PA.nii.gz -coord 3 0 PA.mif
  
We will also add its b-values and b-vectors into the header:

::

  mrconvert PA.mif -fslgrad $PA_BVEC $PA_BVAL - | mrmath - mean mean_b0_PA.mif -axis 3

Next, we extract the b-values from the primary phase-encoded image, and then combine the two with ``mrcat``:

::

  dwiextract dwi_den.mif - -bzero | mrmath - mean mean_b0_AP.mif -axis 3
  mrcat mean_b0_AP.mif mean_b0_PA.mif -axis 3 b0_pair.mif
  
This will create a new image, "b0_pair.mif", which contains both of the average b=0 images for both phase-encoded images.


Putting It All Together: Preprocessing with dwipreproc
******************************************************

We now have everything we need to run the main preprocessing step, which is called by ``dwipreproc``. For the most part, this command is a wrapper that uses FSL commands such as ``topup`` and ``eddy`` to unwarp the data and remove eddy currents. For this tutorial, we will use the following line of code:

::

  dwipreproc dwi_den.mif dwi_den_preproc.mif -nocleanup -pe_dir AP -rpe_pair -se_epi b0_pair.mif -eddy_options " --slm=linear --data_is_shelled"
  
The first arguments are the input and output; the second option, ``-nocleanup``, will keep the temporary processing folder which contains a few files we will examine later. ``-pe_dir AP`` signalizes that the primary phase-encoding direction is anterior-to-posterior, and ``-rpe_pair`` combine with the ``-se_epi`` options indicates that the following input file (i.e., "b0_pair.mif") is a pair of spin-echo images that were acquired with reverse phase-encoding directions. Lastly, ``-eddy_options`` specifies options that are specific to the FSL command ``eddy``. You can visit the `eddy user guide <https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/eddy/UsersGuide>`__ for more options and details about what they do. For now, we will only use the options ``--slm=linear`` (which can be useful for data that was acquired with less than 60 directions) and ``--data_is_shelled`` (which indicates that the diffusion data was acquired with multiple b-values).

This command can take several hours to run, depending on the speed of your computer. For an iMac with 8 processing cores, it takes roughly 2 hours. When it has finished, examine the output to see how eddy current correction and unwarping have changed the data; ideally, you should see more signal restored in regions such as the orbitofrontal cortex, which is particularly susceptible to signal dropout.


Generating a Mask
*****************

As with fMRI analysis, it is useful to create a mask to restrict your analysis only to brain voxels; this will speed up the rest of your analyses.

To do that, it can be useful to run a command beforehand called ``dwibiascorrect``. This can remove inhomogeneities detected in the data that can lead to a better mask estimation. However, it can in some cases lead to a worse estimation; as with all of the preprocessing steps, you should check it before and after each step:

::

  dwibiascorrect -ants dwi_den_preproc.mif dwi_den_preproc_unbiased.mif -bias bias.mif
  
.. note::

  The command above uses the ``-ants`` option, which requires that ANTs be installed on your system. I highly recommending this program, but in case you are unable to install it, you can replace it with the ``-fsl`` option.
  
You are now ready to create the mask with ``dwi2mask``:

::

  dwi2mask dwi_den_preproc_unbiased.mif mask.mif
  
