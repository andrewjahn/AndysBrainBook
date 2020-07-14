.. _MRtrix_09_Scripting:

=============================
MRtrix Tutorial #9: Scripting
=============================

--------------

.. note::

  This section is still under construction; check back soon!

Overview
********

After you've preprocessed and set up a model for a single run for a single subject, you will need to do the same steps for all of the runs for all of the subjects in your dataset. This may seem tedious but doable - we only have forty-two subjects, and one run of diffusion data per subject. You may think that it can be done over the course of a week or so; and you can always assign the task to a couple of Research Assistants.

This attitude is admirable, and if you take this approach you will be able to analyze all of the data eventually. But at some point you will run into two problems:

1. You will find that manually analyzing each run is not only tedious but prone to error, and the probability of making a mistake increases significantly as the number of runs to analyze also increases; and

2. For larger datasets - for example, eighty subjects with five runs each - this approach quickly becomes impractical.

An alternative is to **script** your analysis. Just as an actor has a script which tells him what to say and where to stand and where to move, so can you write a script that tells your computer how to analyze your datasets. This has the double benefit of automating your analyses and being able to analyze datasets of any size - the code for analyzing two subjects or two hundred is virtually identical.

First we will create a template that contains the code needed to analyze a single run, and then we will use a :ref:`for-loop  <Unix_05_ForLoops>` to automate the analysis for all of the runs. The idea is simple; and although the code can be difficult to understand at first, once you become more comfortable with it you will see how you can apply it to any dataset.

.. note::

  The following tutorial complements the Unix tutorial on :ref:`automating the analysis <Unix_09_AutomatingTheAnalysis>`. I recommend reading through that chaper if you need to review the Unix terms for scripting.

Creating the Template
*********************

The simplest way to script our analysis would be to copy and paste all of our commands into a text file, and run it from the command line. This is more or less what we will do; the only change will be to include arguments that the user can fill in with the required files. We can then put this in a loop and run it for all of the subjects in our dataset.

For now, we will do this for a single subject. The scripts will be written in four parts:

1. The first script will perform all of the preprocessing, from denoising to ``tcksift2``;

2. The second script will perform QA checks for each of the major preprocessing outputs;

3. The third script will preprocess the structural images using ``recon-all``; and

4. The last script will create the connectome.

``recon-all`` isn't part of the MRtrix pipeline *per se* - you can use any atlas you want, and you are not restricted to FreeSurfer - but we will include it as a prerequisite for creating the connectome.

..note::

  Before going on, download the preprocessing script `here <https://github.com/andrewjahn/MRtrix_Analysis_Scripts/blob/master/MRtrix_Preproc_AP_Direction.sh>`__. You can download it using ``git``, or by clicking ``Raw``, right-clicking anywhere in the resulting screen, and clicking "Save As". Save it to the directory ``sub-CON03/ses-preop/dwi``. The following sections will explain what each block of code does.


Script 1: Preprocessing
^^^^^^^^^^^^^^^^^^^^^^^

The first script begins with ``bash`` code, which generates a brief help manual if you do not supply the required number of arguments. For example, the block of code at the beginning of the script looks like this:

::

  display_usage() {
    echo "$(basename $0) [Raw Diffusion] [RevPhaseImage] [AP bvec] [AP bval] [PA bvec] [PA bval] [Anatomical]"
    echo "This script uses MRtrix to analyze diffusion data. It requires 7 arguments: 
      1) The raw diffusion image;
      2) The image acquired with the reverse phase-encoding direction;
      3) The bvec file for the data acquired in the AP direction;
      4) The bval file for the data acquired in the AP direction;
      5) The bvec file for the data acquired in the PA direction;
      6) The bval file for the data acquired in the PA direction;
      7) The anatomical image"
    }

    if [ $# -le 6 ]
    then
      display_usage
      exit 1
    fi

  RAW_DWI=$1
  REV_PHASE=$2
  AP_BVEC=$3
  AP_BVAL=$4
  PA_BVEC=$5
  PA_BVAL=$6
  ANAT=$7

These last fields, marked by the numbers 1-7 preceded by a dollar sign (``$``), are the **arguments** that are passed to the script. You will need to input the arguments in the exact order that is listed; for example, the command we will use is this:

::

  bash MRtrix_Preproc_AP_Direction.sh sub-CON03_ses-preop_acq-AP_dwi.nii.gz sub-CON03_ses-preop_acq-PA_dwi.nii.gz \
  sub-CON03_ses-preop_acq-AP_dwi.bvec sub-CON03_ses-preop_acq-AP_dwi.bval \
  sub-CON03_ses-preop_acq-PA_dwi.bvec sub-CON03_ses-preop_acq-PA_dwi.bval \
  ../anat/sub-CON03_ses-preop_T1w.nii.gz

The first two arguments specify the primary and reverse phase-encoded images; the next four arguments point to the .bvec and .bval for the primary and reverse phase-encoded images, respectively; and the last argument is the anatomical image. These arguments will populate the variables in the rest of the script, which is essentially a collation of all of the commands that we used in the previous chapters. For example, the variable ``$RAW_DWI`` will be replaced with the first argument that we supplied, ``sub-CON03_ses-preop_acq-AP_dwi.nii.gz``. 

Copy and paste this command into your terminal and press enter. While it is running, you can read the rest of the preprocessing script (reproduced here for completeness); review it to see how the variables are placed, and how each of the commands will be executed when we run the entire script:

::

  ########################### STEP 1 ###################################
  #	        Convert data to .mif format and denoise	   	               #
  ######################################################################

  # Also consider doing Gibbs denoising (using mrdegibbs). Check your diffusion data for ringing artifacts before deciding whether to use it
  mrconvert $RAW_DWI raw_dwi.mif -fslgrad $AP_BVEC $AP_BVAL
  dwidenoise raw_dwi.mif dwi_den.mif -noise noise.mif

  # Extract the b0 images from the diffusion data acquired in the AP direction
  dwiextract dwi_den.mif - -bzero | mrmath - mean mean_b0_AP.mif -axis 3

  # Extracts the b0 images for diffusion data acquired in the PA direction
  # The term "fieldmap" is taken from the output from Michigan's fMRI Lab; it is not an actual fieldmap, but rather a collection of b0 images with both PA and AP phase encoding
  # For the PA_BVEC and PA_BVAL files, they should be in the follwing format (assuming you extract only one volume):
  # PA_BVEC: 0 0 0
  # PA_BVAL: 0
  mrconvert $FIELDMAP PA.mif # If the PA map contains only 1 image, you will need to add the option "-coord 3 0"
  mrconvert PA.mif -fslgrad $PA_BVEC $PA_BVAL - | mrmath - mean mean_b0_PA.mif -axis 3

  # Concatenates the b0 images from AP and PA directions to create a paired b0 image
  mrcat mean_b0_AP.mif mean_b0_PA.mif -axis 3 b0_pair.mif

  # Runs the dwipreproc command, which is a wrapper for eddy and topup. This step takes about 2 hours on an iMac desktop with 8 cores
  dwifslpreproc dwi_den.mif dwi_den_preproc.mif -nocleanup -pe_dir AP -rpe_pair -se_epi b0_pair.mif -eddy_options " --slm=linear --data_is_shelled"

  # Performs bias field correction. Needs ANTs to be installed in order to use the "ants" option (use "fsl" otherwise)
  dwibiascorrect ants dwi_den_preproc.mif dwi_den_preproc_unbiased.mif -bias bias.mif

  # Create a mask for future processing steps
  dwi2mask dwi_den_preproc_unbiased.mif mask.mif

  ########################### STEP 2 ###################################
  #             Basis function for each tissue type                    #
  ######################################################################

  # Create a basis function from the subject's DWI data. The "dhollander" function is best used for multi-shell acquisitions; it will estimate different basis functions for each tissue type. For single-shell acquisition, use the "tournier" function instead
  dwi2response dhollander dwi_den_preproc_unbiased.mif wm.txt gm.txt csf.txt -voxels voxels.mif

  # Performs multishell-multitissue constrained spherical deconvolution, using the basis functions estimated above
  dwi2fod msmt_csd dwi_den_preproc_unbiased.mif -mask mask.mif wm.txt wmfod.mif gm.txt gmfod.mif csf.txt csffod.mif

  # Creates an image of the fiber orientation densities overlaid onto the estimated tissues (Blue=WM; Green=GM; Red=CSF)
  # You should see FOD's mostly within the white matter. These can be viewed later with the command "mrview vf.mif -odf.load_sh wmfod.mif"
  mrconvert -coord 3 0 wmfod.mif - | mrcat csffod.mif gmfod.mif - vf.mif

  # Now normalize the FODs to enable comparison between subjects
  mtnormalise wmfod.mif wmfod_norm.mif gmfod.mif gmfod_norm.mif csffod.mif csffod_norm.mif -mask mask.mif


  ########################### STEP 3 ###################################
  #            Create a GM/WM boundary for seed analysis               #
  ######################################################################

  # Convert the anatomical image to .mif format, and then extract all five tissue catagories (1=GM; 2=Subcortical GM; 3=WM; 4=CSF; 5=Pathological tissue)
  mrconvert $ANAT anat.mif
  5ttgen fsl anat.mif 5tt_nocoreg.mif

  # The following series of commands will take the average of the b0 images (which have the best contrast), convert them and the 5tt image to NIFTI format, and use it for coregistration.
  dwiextract dwi_den_preproc_unbiased.mif - -bzero | mrmath - mean mean_b0_processed.mif -axis 3
  mrconvert mean_b0_processed.mif mean_b0_processed.nii.gz
  mrconvert 5tt_nocoreg.mif 5tt_nocoreg.nii.gz

  # Uses FSL commands fslroi and flirt to create a transformation matrix for regisitration between the tissue map and the b0 images
  fslroi 5tt_nocoreg.nii.gz 5tt_vol0.nii.gz 0 1 #Extract the first volume of the 5tt dataset (since flirt can only use 3D images, not 4D images)
  flirt -in mean_b0_processed.nii.gz -ref 5tt_vol0.nii.gz -interp nearestneighbour -dof 6 -omat diff2struct_fsl.mat
  transformconvert diff2struct_fsl.mat mean_b0_processed.nii.gz 5tt_nocoreg.nii.gz flirt_import diff2struct_mrtrix.txt
  mrtransform 5tt_nocoreg.mif -linear diff2struct_mrtrix.txt -inverse 5tt_coreg.mif

  #Create a seed region along the GM/WM boundary
  5tt2gmwmi 5tt_coreg.mif gmwmSeed_coreg.mif

  ########################### STEP 4 ###################################
  #                 Run the streamline analysis                        #
  ######################################################################

  # Create streamlines
  # Note that the "right" number of streamlines is still up for debate. Last I read from the MRtrix documentation,
  # They recommend about 100 million tracks. Here I use 10 million, if only to save time. Read their papers and then make a decision
  tckgen -act 5tt_coreg.mif -backtrack -seed_gmwmi gmwmSeed_coreg.mif -nthreads 8 -maxlength 250 -cutoff 0.06 -select 10000000 wmfod_norm.mif tracks_10M.tck

  # Extract a subset of tracks (here, 200 thousand) for ease of visualization
  tckedit tracks_10M.tck -number 200k smallerTracks_200k.tck

  # Reduce the number of streamlines with tcksift
  tcksift2 -act 5tt_coreg.mif -out_mu sift_mu.txt -out_coeffs sift_coeffs.txt -nthreads 8 tracks_10M.tck wmfod_norm.mif sift_1M.txt
  
Script 2: QA Checks
^^^^^^^^^^^^^^^^^^^

Just as with the preprocessing script, the QA script contains all of the quality checks that we did in the previous chapters. You can download it `here <https://github.com/andrewjahn/MRtrix_Analysis_Scripts/blob/master/QC_mrview.sh>`__, and execute it by typing ``bash QC_mrview.sh``. It will use ``mrview`` and ``shview`` to examine the output of each preprocessing step; to proceed to the next QC check, you will need to close the window that is currently open. The contents of the script are reproduced below:

::

  #!/bin/bash

  # These commands are for quality-checking your diffusion data


  ### Quality checks for Step 2 ###

  # Views the voxels used for FOD estimation
  echo "Now viewing the voxels used for FOD estimation (Blue=WM; Green=GM; Red=CSF)"
  mrview dwi_den_preproc_unbiased.mif -overlay.load voxels.mif

  # Views the response functions for each tissue type. The WM function should flatten out at higher b-values, while the other tissues should remain spherical
  echo "Now viewing response function for white matter (press right arrow key to view response function for different shells)"
  shview wm.txt
  echo "Now viewing response function for grey matter"
  shview gm.txt
  echo "Now viewing response function for CSF"
  shview csf.txt

  # Views the FODs overlaid on the tissue types (Blue=WM; Green=GM; Red=CSF)
  mrview vf.mif -odf.load_sh wmfod.mif


  ### Quality checks for Step 3 ###

  # Check alignment of the 5 tissue types before and after alignment (new alignment in red, old alignment in blue)
  mrview dwi_den_preproc_unbiased.mif -overlay.load 5tt_nocoreg.mif -overlay.colourmap 2 -overlay.load 5tt_coreg.mif -overlay.colourmap 1

  # Check the seed region (should match up along the GM/WM boundary)
  mrview dwi_den_preproc_unbiased.mif -overlay.load gmwmSeed_coreg.mif


  ### Quality checks for Step 4 ###

  # View the tracks in mrview
  mrview dwi_den_preproc_unbiased.mif -tractography.load smallerTracks_200k.tck
  
Script 3: Recon-all
^^^^^^^^^^^^^^^^^^^

The FreeSurfer script isn't a separate text file; rather, it is simply two lines of code. If you want to learn more about what these commands do, you can review in the :ref:`FreeSurfer tutorial <FS_03_ReconAll>`:

::

  SUBJECTS_DIR=`pwd`;
  recon-all -i ../anat/sub-CON03_ses-preop_T1w.nii.gz -s sub-CON03_recon -all
  
In which ``sub-CON03`` can be replaced with whichever subject you want to analyze. (Later, we will learn how to replace this in a for-loop). Once recon-all finishes, which may take several hours, you are ready to run the last script.

Script 4: Creating the Connectome
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Creating the connectome takes only a few lines of code. For this tutorial, as mentioned above, we will be using FreeSurfer's **Desikan-Killiany** atlas:

::

  #!/bin/bash

  #Convert the labels of the FreeSurfer parcellation to a format that MRtrix understands. This requires recon-all to have been run on the subject
  labelconvert sub-CON02_recon/mri/aparc+aseg.mgz $FREESURFER_HOME/FreeSurferColorLUT.txt /usr/local/mrtrix3/share/mrtrix3/labelconvert/fs_default.txt sub-CON02_parcels.mif

  #Coregister the parcellation to the grey matter mask
  mrtransform sub-CON02_parcels.mif -interp nearest -linear diff2struct_mrtrix.txt -inverse -datatype uint32 sub-CON02_parcels_coreg.mif

  #Create a whole-brain connectome, representing the streamlines between each parcellation pair in the atlas (in this case, 84x84). The "symmetric" option will make the lower diagonal the same as the upper diagonal, and the "scale_invnodevol" option will scale the connectome by the inverse of the size of the node 
  #tck2connectome -symmetric -zero_diagonal -scale_invnodevol -tck_weights_in sift_1M.txt sub-01_parcels.mif sub-01_parcels.csv -out_assignment assignments_sub-01_parcels.csv
  tck2connectome -symmetric -zero_diagonal -scale_invnodevol -tck_weights_in sift_1M.txt tracks_10M.tck sub-CON02_parcels_coreg.mif sub-CON02_parcels_coreg.csv -out_assignment assignments_sub-CON02_parcels_coreg.csv

Running the Scripts
*******************

I recommend running each script separately in order to check the output from each part, although you may prefer to combine everything into a single master script. In any case, when you have downloaded each of the scripts and placed them in the ``BTC_preop`` folder, you can run the following for-loop to run the preprocessing for subjects 04 and 05 of the control group:

::

  for sub in sub-CON04 sub-CON05; do
    cp *.sh ${sub}/ses-preop/dwi;
    cd ${sub}/ses-preop/dwi;
    bash MRtrix_Preproc_AP_Direction.sh ${sub}_ses-preop_acq-AP_dwi.nii.gz ${sub}_ses-preop_acq-PA_dwi.nii.gz \
    ${sub}_ses-preop_acq-AP_dwi.bvec ${sub}_ses-preop_acq-AP_dwi.bval \
    ${sub}_ses-preop_acq-PA_dwi.bvec ${sub}_ses-preop_acq-PA_dwi.bval \
    ../anat/${sub}_ses-preop_T1w.nii.gz
    cd ../../..;
  done
  
