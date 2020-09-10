.. _ML_02_Haxby_Intro_Download:

===============================================
Machine Learning Tutorial #2: The Haxby Dataset
===============================================


-----------

Overview
********

During the 1990s, fMRI studies focused on activation - which region of the brain responded to particular stimuli. fMRI was a new method, and researchers were able to use it to non-invasively map which regions of the brain responded to touch, pictures, noises, and other basic stimuli. These experiments measured the *amplitude" of the BOLD response to each of the different stimuli, and then compared the amplitudes between conditions to see which one elicited greater brain activity.

In 2001, James Haxby conducted an experiment that instead focused on *patterns* of activity instead of the amplitude itself. If we take a 2x2 grid of squares and assign a number in each square representing the BOLD response, certain stimuli may elicit a specific pattern that can be detected by a classifier. If that pattern is consistent and unique, we will be able to distinguish that pattern from that elicited by another stimulus.

Downloading the Data
********************

The dataset can be downloaded `here <https://openneuro.org/datasets/ds000105/versions/00001>`__ from the OpenNeuro website. If clicking on the Download button doesn't work, use the AWS option by following the instructions located `here <https://aws.amazon.com/cli/>`__, and then typing:

::

  aws s3 sync --no-sign-request s3://openneuro.org/ds000105 ds000105-download/
  
Once you have downloaded the data, move it to your Desktop and rename it by typing:

::

  mv ~/Downloads/ds000105 ~/Desktop/Haxby_Data


Preprocessing the Data
**********************

The preprocessing steps are mostly the same as with analyzing a typical fMRI dataset; however, we will omit the **smoothing** step to avoid blurring the signal between voxels. While this is useful in univariate analyses that focus on averaging together nearby signal, in this case we want to keep the signal in each voxel as distinct as possible. This will help the classifier distinguish between the patterns elicited by different stimuli, if the patterns exist.

Creating the Timing Files
^^^^^^^^^^^^^^^^^^^^^^^^^

Just as with our traditional fMRI analyses, we will need to create timing files that indicate which condition was presented at what time. These are found in the ``func`` folder for each subject with one timing file per run, with a ".tsv" extension. Within the file ``sub-1_task_objectviewing_run-01_events.tsv`` for example, we see the following lines:

::

  onset   duration        trial_type
  12.000  0.500   scissors
  14.000  0.500   scissors
  16.000  0.500   scissors
  18.000  0.500   scissors
  20.000  0.500   scissors
  22.000  0.500   scissors
  24.000  0.500   scissors
  26.000  0.500   scissors
  28.000  0.500   scissors
  30.000  0.500   scissors
  32.000  0.500   scissors
  34.000  0.500   scissors
  48.000  0.500   face
  50.000  0.500   face
  52.000  0.500   face
  54.000  0.500   face
  56.000  0.500   face
  58.000  0.500   face
  60.000  0.500   face
  62.000  0.500   face
  64.000  0.500   face
  66.000  0.500   face
  68.000  0.500   face
  70.000  0.500   face
  
The timings go on to include all of the other conditions, of which there are eight total: Bottles, Cats, Chairs, Faces, Houses, Scissors, Scrambled Pictures (labeled as ScrambledPix), and Shoes. Each condition was presented in a block containing several individual trials of the same stimulus type presented one after another, with each block lasting for 24 seconds. In order to write these timings into a format that AFNI understands, we will use the following code in the ``func`` directory for subject 1:

::

  #!/bin/bash
  for cond in bottle cat chair face house scissors scrambledpix shoe; do
          for i in `seq -w 1 12`; do
                  cat sub-1_task-objectviewing_run-${i}_events.tsv | awk -v i="$cond" '{if ($3==i) print $1}' | head -1 >> ${cond}.1D
          done
  done
  
This loops over all of the runs in the experiment (12 total) and all of the conditions, extracting the first timestamp for the condition that is presented and placing it in a corresponding 1D file. For example, the timing file for ``bottle.1D`` looks like this:

::

  228.000
  192.000
  156.000
  228.000
  84.000
  228.000
  264.000
  156.000
  228.000
  264.000
  120.000
  12.000
  
Later, we will use options in the regression command ``3dDeconvolve`` to indicate that these are **local times**, one timestamp per run, and that each one lasted for 24 seconds. For now, run the above code to create a 1D timing file for each condition. When it is done, you should see something like this in the folder ``sub-1/func``:

.. figure:: 02_TimingFiles.png


Creating the AFNI_proc File
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Next, we will create a preprocessing file using AFNI's afni_proc.py command. Note that we include all of the usual preprocessing blocks *except* blurring, which smooths the data:

::

  #!/bin/tcsh

set subj=sub-1

  afni_proc.py -subj_id $subj -script proc.$subj -scr_overwrite -blocks tshift                                                  \
     align tlrc volreg mask scale regress -copy_anat                                                                  \
     $PWD/{$subj}/anat/{$subj}_T1w.nii.gz                     \
     -dsets                                                                                                                \
     $PWD/{$subj}/func/{$subj}_task-objectviewing_run-*_bold.nii.gz \
     -tcat_remove_first_trs 0 -align_opts_aea -giant_move -tlrc_base                                                       \
     MNI_avg152T1+tlrc -volreg_align_to MIN_OUTLIER -volreg_align_e2a                                                      \
     -volreg_tlrc_warp -regress_local_times -regress_stim_types IM -regress_apply_mask -regress_stim_times                                                                 \
     $PWD/{$subj}/func/bottle.1D                          \
     $PWD/{$subj}/func/cat.1D                        \
     $PWD/{$subj}/func/chair.1D                        \
     $PWD/{$subj}/func/face.1D                        \
     $PWD/{$subj}/func/house.1D                        \
     $PWD/{$subj}/func/scissors.1D                        \
     $PWD/{$subj}/func/scrambledpix.1D                        \
     $PWD/{$subj}/func/shoe.1D                        \
     -regress_stim_labels bottle cat chair face house scissors scrambledpix shoe -regress_basis 'BLOCK(24,1)'                                                \
     -regress_censor_motion 0.3 -regress_motion_per_run -regress_opts_3dD                                                  \
     -local_times -jobs 8 -regress_make_ideal_sum sum_ideal.1D -regress_est_blur_epits                                                  \
     -regress_est_blur_errts -regress_run_clustsim no
     
As we saw in the previous tutorial analyzing the Brown data, we will use the option ``-regress_stim_types IM`` to Individually Modulate each condition; that is, estimate a separate beta map for each trial within that condition. These will then be used as training and testing maps for our classifier. Also note that we use the basis function BLOCK to model each condition as a 24-second boxcar regressor.

Now run this script from the folder ``Haxby_Data`` which contains all of the individual subject folders. You can either copy and paste the code above directly into the terminal, or place it into a text file, save it as "Haxby_proc.sh", and type:

::

  tcsh Haxby_proc.sh
  
This in turn will generate a file called "proc.sub-1". The only edit I would make is on line 148, by adding the ``-init_xform`` option to the end of the ``@auto_tlrc`` command:

::

  @auto_tlrc -base MNI_avg152T1+tlrc -input sub-1_T1w_ns+orig -no_ss -init_xform AUTO_CENTER
  
Then, run the script by typing:

  tcsh -xef proc.sub-1
  
After about half an hour, you should see all of the files output into the folder ``sub-1.results``. You can do QA checks by navigating into the directory and typing:

::

  afni_open -b sub-1.results/QC_sub-1/index.html
  
To check registration, normalization, and any volumes censored due to motion.

