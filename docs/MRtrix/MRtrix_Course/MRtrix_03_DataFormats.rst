.. _MRtrix_03_DataFormats:

================================
MRtrix Tutorial #3: Data Formats
================================

---------------

Overview
--------------

MRtrix uses its own format for storing and displaying imaging data. If you've already gone through the tutorials on the major fMRI software packages, such as SPM, FSL, and AFNI, you may remember that all of them can read and write images in NIFTI format. (AFNI by default will write files in its own BRIK/HEAD format unless you specify that your output should have a .nii extension, but it is the sole exception.) MRtrix is also able to read raw data in NIFTI format, but will output its files in MRtrix format, signalized with a ``.mif`` extension.

To see how this works, navigate to the folder ``sub-CON01/ses-preop/dwi``, which contains your diffusion data. One of the first steps for preprocessing your data is to convert the diffusion data into a format that MRtrix understands; we will use the command ``mrconvert`` to combine the raw diffusion data with its corresponding ``.bval`` and ``.bvec`` files, so that we can use the combined file for future preprocessing steps:

::

  mrconvert sub-CON01_ses-preop_dwi_sub-CON01_ses-preop_acq-AP_dwi.nii.gz sub-01_dwi.mif -fslgrad sub-CON01_ses-preop_dwi_sub-CON01_ses-preop_acq-AP_dwi.bvec sub-CON01_ses-preop_dwi_sub-CON01_ses-preop_acq-AP_dwi.bval
  
This command requires three arguments: The input, which is the raw DWI file in the AP directory; an output file, which we will call sub-01_dwi.mif to make it more compact and easier to read; and ``-fslgrad``, which requires the corresponding .bvec and .bval files (in that order).

The output image, ``sub-01_dwi.mif``, can be read with the command ``mrinfo``:

::

  mrinfo sub-01_dwi.mif
  
