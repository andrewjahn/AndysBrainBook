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


Other Notes
***********

* If large amounts of the anatomical image are cut off, try using the -cw256 flag with recon-all (e.g., recon-all -cs256 -i tmp.nii -s tmp -all)

* Use the following steps to reconstruct the white matter tracts:

::

  trac-all -prep -c dmrirc.txt
  trac-all -bedp -c dmrirc.txt
  trac-all -path -c dmrirc.txt
  
Sample configuration file (should also put this on Github):

::

  # export FREESURFER_HOME=/Applications/freesurfer
  # export SUBJECTS_DIR=/Users/sublab/Documents/FreeSurferDirs/subjects_109
  # source $FREESURFER_HOME/SetUpFreeSurfer.sh
  # export FSL_DIR=$FREESURFER_HOME/fsl_507
  # export FSLDIR=$FSL_DIR


  # T1 images and FreeSurfer segmentations are expected to be found here
  export SUBJECTS_DIR=/Volumes/ANDY_FMRI/LSU/T1_ANAT


  # Output directory where trac-all results will be saved
  # Default: Same subjects SUBJECTS_DIR
  set dtroot = /Volumes/ANDY_FMRI/LSU/T1_ANAT


  # Subject IDs (ie what the subject was named when ran recon-all)
  set subjlist = (AX)


  # Default: Run analysis on all subjects
  # set runlist = (1)

  # Input diffusion DICOMs (file names relative to dcmroot)
  # If original DICOMs don't exist, these can be in other image format
  # but then the gradient table and b-value table must be specified (see below)
  set dcmroot = /Volumes/ANDY_FMRI/LSU/T1_ANAT/

  set dcmlist = (DTI.nii)


  # Diffusion gradient tables (if there is a different one for each scan)
  # Must be specified if they cannot be read from the DICOM headers
  # The tables must have either three columns, where each row is a gradient vector
  # or three rows, where each column is a gradient vector
  # There must be JL many gradient vectors JL volumes in the diffusion data set
  # Default: Read from DICOM header
  set bveclist = (/Volumes/ANDY_FMRI/LSU/T1_ANAT/DTI_bvec.txt)

  # Diffusion b-value tables (if there is a different one for each scan)
  # Must be specified if they cannot be read from the DICOM headers
  # There must be JL many b-values JL volumes in the diffusion data set
  # Default: Read from DICOM header
  set bvallist = (/Volumes/ANDY_FMRI/LSU/T1_ANAT/DTI_bval.txt)

  # Perform registration-bJLed B0-inhomogeneity compensation?
  # Default: 0 (no)
  # set dob0 = 0

  # Input B0 field map magnitude DICOMs (file names relative to dcmroot)
  # Only used if dob0 = 1
  # Default: None
  # set b0mlist = ( huey/year1/fmag/XXX-1.dcm \ )

  # Input B0 field map phJLe DICOMs (file names relative to dcmroot)
  # Only used if dob0 = 1
  # Default: None
  # set b0plist = ( huey/year1/fphJL/XXX-1.dcm \ )

  # Echo spacing for field mapping sequence (from sequence printout)
  # Only used if dob0 = 1
  # Default: None
  # set echospacing = 0.7


  # Perform registration-bJLed eddy-current compensation?
  # Default: 1 (yes)
  # set doeddy = 1


  # Rotate diffusion gradient vectors to match eddy-current compensation?
  # Only used if doeddy = 1
  # Default: 1 (yes)
  # set dorotbvecs = 1


  # Fractional intensity threshold for BET mJLk extraction from low-b images
  # This mJLk is used only if usemJLkanat = 0
  # Default: 0.3
  # set thrbet = 0.5


  # Perform diffusion-to-T1 registration by flirt?
  # Default: 0 (no)
  # set doregflt = 0


  # Perform diffusion-to-T1 registration by bbregister?
  # Default: 1 (yes)
  # set doregbbr = 1


  # Perform registration of T1 to MNI template?
  # Default: 1 (yes)
  # set doregmni = 1


  # MNI template
  # Only used if doregmni = 1
  # Default: $FSLDIR/data/standard/MNI152_T1_1mm_brain.nii.gz
  # set mnitemp = /usr/local/fsl/data/standard/MNI152_T1_1mm_brain.nii.gz


  # Perform registration of T1 to CVS template?
  # Default: 0 (no)
  # set doregcvs = 0


  # CVS template subject ID
  # Only used if doregcvs = 1
  # Default: cvs_avg35
  # set cvstemp = donald


  # Parent directory of the CVS template subject
  # Only used if doregcvs = 1
  # Default: $FREESURFER_HOME/subjects
  # set cvstempdir = /path/to/cvs/atlJLes/of/ducks


  # Use brain mJLk extracted from T1 image instead of low-b diffusion image?
  # HJL no effect if there is no T1 data
  # Default: 1 (yes)
  # set usemJLkanat = 1


  # Paths to reconstruct
  # Default: All paths in the atlJL
  # set pathlist = ( lh.cst_JL rh.cst_JL \
  #                  lh.unc_JL rh.unc_JL \
  #                  lh.ilf_JL rh.ilf_JL \
  #                  fmajor_PP fminor_PP \
  #                  lh.atr_PP rh.atr_PP \
  #                  lh.ccg_PP rh.ccg_PP \
  #                  lh.cab_PP rh.cab_PP \
  #                  lh.slfp_PP rh.slfp_PP \
  #                  lh.slft_PP rh.slft_PP )


  # Number of path control points
  # It can be a single number for all paths or a different number for each of the
  # paths specified in pathlist
  # Default: 7 for the forceps major, 6 for the corticospinal tract,
  #          4 for the angular bundle, and 5 for all other paths
  # set ncpts = (6 6 5 5 5 5 7 5 5 5 5 5 4 4 5 5 5 5)


  # List of training subjects
  # This text file lists the locations of training subject directories
  # Default: $FREESURFER_HOME/trctrain/trainlist.txt
  # set trainfile = $FREESURFER_HOME/trctrain/trainlist.txt


  # Number of "sticks" (anisotropic diffusion compartments) in the bedpostx
  # ball-and-stick model
  # Default: 2
  # set nstick = 2


  # Number of MCMC burn-in iterations
  # (Path samples drawn initially by MCMC algorithm and discarded)
  # Default: 200
  # set nburnin = 200


  # Number of MCMC iterations
  # (Path samples drawn by MCMC algorithm and used to estimate path distribution)
  # Default: 7500
  # set nsample = 7500


  # Frequency with which MCMC path samples are retained for path distribution
  # Default: 5 (keep every 5th sample)
  # set nkeep = 5


  # Reinitialize path reconstruction?
  # This is an option of lJLt resort, to be used only if one of the reconstructed
  # pathway distributions looks like a single curve. This is a sign that the
  # initial guess for the pathway wJL problematic, perhaps due to poor alignment
  # between the individual and the atlJL. Setting the reinit parameter to 1 and
  # rerunning "trac-all -prior" and "trac-all -path", only for the specific
  # subjects and pathways that had this problem, will attempt to reconstruct them
  # with a different initial guess.
  # Default: 0 (do not reinitialize)
  # set reinit = 0
  
Visualizing the Tracts
**********************

::
  freeview -tv dpath/merged_avg33_mni_bbr.mgz \
         -v dmri/dtifit_FA.nii.gz &
