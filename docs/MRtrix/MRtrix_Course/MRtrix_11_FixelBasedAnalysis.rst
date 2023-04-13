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

  Figure 1 from Raffelt et al., 2017


Preparing the Data for Fixel-Based Analysis
*******************************************

Some of the steps for doing a Fixel-Based Analysis are similar to what was done in the previous chapters of this module. There are some significant differences, however, once the Fiber Orientations are estimated and then warped to a template.

If you are analyzing a large number of subjects - for example, several dozen or more - you will either need a computer with at least a couple of hundred gigabytes of memory, and ideally multiple processors. Fixel-based analysis can be done on a local machine, but it can take a long time; several days, depending on how many subjects you have. Regardless, you can process these images by using MRtrix's ``for_each`` command (details about the command can be found `here <https://mrtrix.readthedocs.io/en/dev/reference/commands/for_each.html>`__). To illustrate how to use this batch command, we will take a relatively simple case of denoising the raw diffusion-weighted data.

To begin, we will organize our data so that each subject folder contains converted raw data called ``dwi.mif``. For example, if we have three subjects from the BTC_Preop dataset, we can create a new sub-folder called ``Fixel_Analysis``. Within that folder, create another folder called ``subjects``, which contains three sub-folders, ``sub-01``, ``sub-02``, and ``sub-03``. Assuming that each of these folders contain each subject's corresponding raw diffusion data and bvals and bvecs files, you can convert them by typing the following code from the ``subjects`` folder:

::

  for i in sub-01 sub-02 sub-03; do
    mrconvert *.nii.gz dwi.mif -fslgrad *.bvec *bval
    rm *.nii.gz
  done
  
This will create a new raw diffusion-weighted image, which contains the b-vectors and b-values in its header.

The next preprocessing steps can be found on `this webpage <https://mrtrix.readthedocs.io/en/latest/fixel_based_analysis/mt_fibre_density_cross-section.html>`__, on the MRtrix website. They explain each of the steps in detail, so we will not repeat all of the explanations here. Let us instead focus on the first preprocessing step, denoising, to illustrate how the ``for_each`` command works. For example, if we run the following code from the ``subjects`` directory:

::
  
  for_each * : dwidenoise IN/dwi.mif IN/dwi_denoised.mif
  
The code to the left of the colon (``:``) means to loop over every item captured by the ``*`` wildcard. (Note that if there are any other files besides the subject folders in this directory, such as text files, that the command will throw an error, since it is trying to applyg a preprocessing step to a non-diffusion file.) Each of the items are loaded into the ``IN`` keyword; for the first subject, for example, this will expand to:

::

  dwidenoise sub-01/dwi.mif sub-01/dwi_denoised.mif
  
  
Which takes sub-01's dwi.mif file, denoises it, and places the output file, ``dwi_denoised.mif``, in the sub-01 folder. This is the same procedure that is used for all of the other ``for_each`` commands listed in the tutorial. One variation that you should be aware of is the ``PRE`` keyword, which is the input item stripped of its extension.


Running the Preprocessing Steps
*******************************

You can either adapt the commands from the MRtrix tutorial to you data structure, or, assuming that you have the subjects organized with a single ``dwi.mif`` file in each folder, you can copy and paste the following code below (note that this omits bias field correction, which, in my experience, can sometimes results in worse brain mask estimation later on):

::

  for_each * : dwidenoise IN/dwi.mif IN/dwi_denoised.mif
  for_each * : mrdegibbs IN/dwi_denoised.mif IN/dwi_denoised_unringed.mif -axes 0,1
  for_each * : dwifslpreproc IN/dwi_denoised_unringed.mif IN/dwi_denoised_unringed_preproc.mif -rpe_none -pe_dir AP
  for_each * : dwi2response dhollander IN/dwi_denoised_unringed_preproc.mif IN/response_wm.txt IN/response_gm.txt IN/response_csf.txt
  responsemean */response_wm.txt ../group_average_response_wm.txt
  responsemean */response_gm.txt ../group_average_response_gm.txt
  responsemean */response_csf.txt ../group_average_response_csf.txt
  for_each * : mrgrid IN/dwi_denoised_unringed_preproc_unbiased.mif regrid -vox 1.25 IN/dwi_denoised_unringed_preproc_unbiased_upsampled.mif
  for_each * : dwi2mask IN/dwi_denoised_unringed_preproc_unbiased_upsampled.mif IN/dwi_mask_upsampled.mif
  for_each * : dwi2fod msmt_csd IN/dwi_denoised_unringed_preproc_unbiased_upsampled.mif ../group_average_response_wm.txt IN/wmfod.mif ../group_average_response_gm.txt IN/gm.mif  ../group_average_response_csf.txt IN/csf.mif -mask IN/dwi_mask_upsampled.mif
  for_each * : mtnormalise IN/wmfod.mif IN/wmfod_norm.mif IN/gm.mif IN/gm_norm.mif IN/csf.mif IN/csf_norm.mif -mask IN/dwi_mask_upsampled.mif
  mkdir -p ../template/fod_input
  mkdir ../template/mask_input
  for_each * : ln -sr IN/wmfod_norm.mif ../template/fod_input/PRE.mif
  for_each * : ln -sr IN/dwi_mask_upsampled.mif ../template/mask_input/PRE.mif
  population_te mplate ../template/fod_input -mask_dir ../template/mask_input ../template/wmfod_template.mif -voxel_size 1.25
  for_each * : mrregister IN/wmfod_norm.mif -mask1 IN/dwi_mask_upsampled.mif ../template/wmfod_template.mif -nl_warp IN/subject2template_warp.mif IN/template2subject_warp.mif
  for_each * : mrtransform IN/dwi_mask_upsampled.mif -warp IN/subject2template_warp.mif -interp nearest -datatype bit IN/dwi_mask_in_template_space.mif
  mrmath */dwi_mask_in_template_space.mif min ../template/template_mask.mif -datatype bit
  fod2fixel -mask ../template/template_mask.mif -fmls_peak_value 0.06 ../template/wmfod_template.mif ../template/fixel_mask
  for_each * : mrtransform IN/wmfod_norm.mif -warp IN/subject2template_warp.mif -reorient_fod no IN/fod_in_template_space_NOT_REORIENTED.mif
  for_each * : fod2fixel -mask ../template/template_mask.mif IN/fod_in_template_space_NOT_REORIENTED.mif IN/fixel_in_template_space_NOT_REORIENTED -afd fd.mif
  for_each * : fixelreorient IN/fixel_in_template_space_NOT_REORIENTED IN/subject2template_warp.mif IN/fixel_in_template_space
  for_each * : fixelcorrespondence IN/fixel_in_template_space/fd.mif ../template/fixel_mask ../template/fd PRE.mif
  for_each * : warp2metric IN/subject2template_warp.mif -fc ../template/fixel_mask ../template/fc IN.mif
  mkdir ../template/log_fc
  cp ../template/fc/index.mif ../template/fc/directions.mif ../template/log_fc
  for_each * : mrcalc ../template/fc/IN.mif -log ../template/log_fc/IN.mif
  mkdir ../template/fdc
  cp ../template/fc/index.mif ../template/fdc
  cp ../template/fc/directions.mif ../template/fdc
  for_each * : mrcalc ../template/fd/IN.mif ../template/fc/IN.mif -mult ../template/fdc/IN.mif
  cd ../template
  tckgen -angle 22.5 -maxlen 250 -minlen 10 -power 1.0 wmfod_template.mif -seed_image template_mask.mif -mask template_mask.mif -select 20000000 -cutoff 0.06 tracks_20_million.tck
  tcksift tracks_20_million.tck wmfod_template.mif tracks_2_million_sift.tck -term_number 2000000
  fixelconnectivity fixel_mask/ tracks_2_million_sift.tck matrix/
  fixelfilter fd smooth fd_smooth -matrix matrix/
  fixelfilter log_fc smooth log_fc_smooth -matrix matrix/
  fixelfilter fdc smooth fdc_smooth -matrix matrix/

.. note::

  Sometimes the command ``dwi2mask`` may fail to cover the entire brain, especially pockets of cerebrospinal fluid. In that case, you can replace the ``dwi2mask`` command with FSL's ``bet2`` command, which will require converting the mask to NIFTI format and then back to .mif format:
  
.. note::

  On Macintosh operating systems, the command ``ln -sr`` may not work (it should work on most Linux systems). In that case, copy the ``wmfod*.mif`` files into the ``template/fod_input`` folder, and copy the files ``dwi_mask_upsampled*.mif`` into the ``template/mask_input`` folder. You may have to give each of the files a unique subject ID.
  
  ::
  
    mrconvert -force dwi_denoised_unringed_preproc_upsampled.mif tmp.nii
    bet2 tmp.nii tmp -m -f 0.2
    mrconvert -force tmp_mask.nii.gz dwi_mask_upsampled.mif
    rm tmp*
    
  Make sure to check the mask as we did in the previous tutorials of this walkthrough, to ensure that there are no holes in the mask. You may have to change the value after the -f option to generate a good whole-brain mask that covers all of the voxels of the brain. Using this approach may resolve any "balance factor" errors with ``mtnormalise``, especially if one or more of the tissue types is empty.

Creating The GLM
****************

  The last few steps in the tutorial require a design matrix and a contrast matrix, similar to those that are created with FSL (for examples, see `this page <https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/GLM>`__). For a simple effect averaged across all of the participants Fixel-based metrics, we would create a column of all 1's, such as the following:
  
::

  1
  1
  1

And save it into a file called ``design_matrix.txt`` (or whatever label you find most useful). We would also create an accompanying contrast matrix, which would just contain:

::

  1
  
And save it into a file called ``contrast_matrix.txt``, in this example. The above text files specify that we are weighting all of the subjects equally, and giving them a contrast value of 1 to average across all of their values for each fixel. We can then run these simple effect analyses on each of the fixel-based metrics - FD, FC, and FDC:


  fixelcfestats fd_smooth/ files.txt design_matrix.txt contrast_matrix.txt matrix/ stats_fd/
  fixelcfestats log_fc_smooth/ files.txt design_matrix.txt contrast_matrix.txt matrix/ stats_log_fc/
  fixelcfestats fdc_smooth/ files.txt design_matrix.txt contrast_matrix.txt matrix/ stats_fdc/
  
This will generate each metric's corresponding stats folder, with each one containing files that represent different statistics. For example, we can load the file ``wmfod_template.mif`` in mrview as an underlay, and then click on ``Tool -> Fixel plot``; to load, for example, the ``Zstat.mif`` file, click on the folder icron at the top of the Fixel plot panel, navigate to ``stats_fdc``, and select the file ``Zstat.mif``. You should see something like this:

.. figure:: 11_Zstat_FDC_Example.png

By default, there will be a colorscale bar in the viewing panel showing the minimum and maximum Z-statistic in this image; in our case, the maximum Z-statistic is 5.33, indicating the highest Z-statistic for the FDC values. If we zoom into a region such as the left superior longitudinal fasciculus, we can see each fixel composed of three orthogonal directions, changing in orientation as we move along the different fiber bundles, and each vector of the fixel color-coded by its strength:

.. figure:: 11_FDC_Directions.png

You can also load the file ``fwe_1mpvalue.mif``, which will show a 1-p map of significant fixels, which can be thresholded at 0.95 to show only those fixels that pass a significance threshold of p=0.05. Given that we only have three subjects, it's unlikely that we have any significant fixels, and they wouldn't mean much for a simple effects analysis in any case. To look at contrasts between groups, on the other hand, we will analyze the entire dataset on a computing cluster, such as the University of Michigan's Great Lakes supercomputer.


Fixel-Based Analysis on the Supercomputing Cluster
**************************************************

