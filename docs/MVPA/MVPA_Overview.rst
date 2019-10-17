.. _MVPA_Overview:

===================================================
Introduction to Multi-Voxel Pattern Analysis (MVPA)
===================================================

---------

Overview
********

This contains notes on a sample MVPA dataset I am currently analyzing; this is still under construction. The following is based on a template I found on a Brown University website `here <https://www.brown.edu/carney/mri/researchers/analysis-pipelines/mvpa>`__.


Setting up the Script
*********************

Create the script with uber_subject.py, as in the :ref:`AFNI tutorials <AFNI_Intermezzo_Uber_Subject>`. When you have generated the script, change the following:

1. Add ``-init_xform AUTO_CENTER`` after the @auto_tlrc command;
2. Add ``-force_TR 0.8``, ``-global_times``, and ``-mask mask_group+tlrc`` to the 3dDeconvolve command;
3. Change the basis function to stim_times_IM (if you haven't already done so in the uber_subject.py script).


Run the script, which may take 2-3 hours. The regression for the LipRead dataset will take a long time, since it is a large dataset with many regressors being estimated.


.. note::

  If the functional or anatomical files are labeled as though they are in TLRC space, although they haven't been normalized, use the following command to put them in ORIG space:
  
  3drefit -space ORIG run_01.nii
  
  And so on, for all of the images that need to be reformatted.

Extracting the Individual Betas
*******************************

Next, the beta estimates will need to be extracted for each trial for each run separately. We will then use 4 of the runs as training data, and the last 2 as test datasets. (Not sure about these exact numbers; maybe they can be modified, e.g., 5 training runs and 1 test run.) The following script, when it is finalized, will be uploaded on Github and called ``01_extractBetas.sh``:

::

  #!/bin/bash

  # This script extracts the betas from the lipreading study for an MVPA analysis

  # Extract the individual beta estimates for each trial

  3dTcat -prefix aud_phoneme_allBetas.nii stats_IM.dev03+tlrc'[1-233(2)]'
  3dTcat -prefix aud_word_allBetas.nii stats_IM.dev03+tlrc'[236-468(2)]'
  3dTcat -prefix vis_phoneme_allBetas.nii stats_IM.dev03+tlrc'[471-703(2)]'
  3dTcat -prefix vis_word_allBetas.nii stats_IM.dev03+tlrc'[706-938(2)]'

  # Extract each run's individual betas

  ### aud_phoneme ###
  3dTcat -prefix aud_phoneme_run1_Betas.nii aud_phoneme_allBetas.nii'[0-24]'
  3dTcat -prefix aud_phoneme_run2_Betas.nii aud_phoneme_allBetas.nii'[25-39]'
  3dTcat -prefix aud_phoneme_run3_Betas.nii aud_phoneme_allBetas.nii'[40-62]'
  3dTcat -prefix aud_phoneme_run4_Betas.nii aud_phoneme_allBetas.nii'[63-83]'
  3dTcat -prefix aud_phoneme_run5_Betas.nii aud_phoneme_allBetas.nii'[84-96]'
  3dTcat -prefix aud_phoneme_run6_Betas.nii aud_phoneme_allBetas.nii'[97-116]'


  ### aud_word ###
  3dTcat -prefix aud_word_run1_Betas.nii aud_word_allBetas.nii'[0-15]'
  3dTcat -prefix aud_word_run2_Betas.nii aud_word_allBetas.nii'[16-40]'
  3dTcat -prefix aud_word_run3_Betas.nii aud_word_allBetas.nii'[41-62]'
  3dTcat -prefix aud_word_run4_Betas.nii aud_word_allBetas.nii'[63-80]'
  3dTcat -prefix aud_word_run5_Betas.nii aud_word_allBetas.nii'[81-95]'
  3dTcat -prefix aud_word_run6_Betas.nii aud_word_allBetas.nii'[96-116]'

  ### vis_phoneme ###
  3dTcat -prefix vis_phoneme_run1_Betas.nii vis_phoneme_allBetas.nii'[0-18]'
  3dTcat -prefix vis_phoneme_run2_Betas.nii vis_phoneme_allBetas.nii'[19-36]'
  3dTcat -prefix vis_phoneme_run3_Betas.nii vis_phoneme_allBetas.nii'[37-53]'
  3dTcat -prefix vis_phoneme_run4_Betas.nii vis_phoneme_allBetas.nii'[54-76]'
  3dTcat -prefix vis_phoneme_run5_Betas.nii vis_phoneme_allBetas.nii'[77-103]'
  3dTcat -prefix vis_phoneme_run6_Betas.nii vis_phoneme_allBetas.nii'[104-116]'

  ### vis_word ###
  3dTcat -prefix vis_word_run1_Betas.nii vis_word_allBetas.nii'[0-17]'
  3dTcat -prefix vis_word_run2_Betas.nii vis_word_allBetas.nii'[18-37]'
  3dTcat -prefix vis_word_run3_Betas.nii vis_word_allBetas.nii'[38-53]'
  3dTcat -prefix vis_word_run4_Betas.nii vis_word_allBetas.nii'[54-69]'
  3dTcat -prefix vis_word_run5_Betas.nii vis_word_allBetas.nii'[70-92]'
  3dTcat -prefix vis_word_run6_Betas.nii vis_word_allBetas.nii'[93-116]'
  
  
Creating the Training and Testing Datasets
******************************************

Now that we have our betas separated by run, we can create a single training block and a single testing block, using the ``3dTcat`` command (in a script to be labeled ``02_createTrainTestDatasets.sh``):

::

  #!/bin/bash

  # This script creates training and testing blocks for each condition

  # First, extract the Betas for each condition for runs 1-4 and put them into a single training block

  3dTcat -prefix aud_phoneme_Train.nii aud_phoneme_run1_Betas.nii aud_phoneme_run2_Betas.nii aud_phoneme_run3_Betas.nii aud_phoneme_run4_Betas.nii
  3dTcat -prefix aud_word_Train.nii aud_word_run1_Betas.nii aud_word_run2_Betas.nii aud_word_run3_Betas.nii aud_word_run4_Betas.nii
  3dTcat -prefix vis_phoneme_Train.nii vis_phoneme_run1_Betas.nii vis_phoneme_run2_Betas.nii vis_phoneme_run3_Betas.nii vis_phoneme_run4_Betas.nii
  3dTcat -prefix vis_word_Train.nii vis_word_run1_Betas.nii vis_word_run2_Betas.nii vis_word_run3_Betas.nii vis_word_run4_Betas.nii

  # Then concatenate all of these training blocks into a single training block

  3dTcat -prefix trainBlock.nii aud_phoneme_Train.nii aud_word_Train.nii vis_phoneme_Train.nii vis_word_Train.nii


  # Now create the testing set, consisting of the last two runs (5-6)

  3dTcat -prefix aud_phoneme_Test.nii aud_phoneme_run5_Betas.nii aud_phoneme_run6_Betas.nii
  3dTcat -prefix aud_word_Test.nii aud_word_run5_Betas.nii aud_word_run6_Betas.nii
  3dTcat -prefix vis_phoneme_Test.nii vis_phoneme_run5_Betas.nii vis_phoneme_run6_Betas.nii
  3dTcat -prefix vis_word_Test.nii vis_word_run5_Betas.nii vis_word_run6_Betas.nii

  # And concatenate those into a single test block
  
  3dTcat -prefix testBlock.nii aud_phoneme_Test.nii aud_word_Test.nii vis_phoneme_Test.nii vis_word_Test.nii
  
  
Running the MVPA Test
*********************

Lastly, we will use AFNI's ``3dsvm`` command to train a classifier, and then use that classifier to determine which condition an "unknown" beta belongs to (``03_TrainTestAlgorithm.sh``):

::

  #!/bin/bash

  3dsvm -trainvol trainBlock.nii \
          -trainlabels trainLabels.1D \
          -model trainSet_noise.model.nii \
          -mask noiseROI.nii

  3dsvm -testvol testBlock.nii \
          -model trainSet_noise.model.nii \
          -classout \
          -predictions exemplar_noise
          
 
In this example, I created a "noiseROI" located outside the brain; in this case, the classification accuracy should be around chance, or 25% (since there are 4 possible conditions in this dataset). The output, "exemplar_noise", can be compared against the "trainLabels.1D" file. If the numbers are the same, it was a match; if they are different, then it was a misclassification. The correct classifications are summed up and divided by the total number of classifications that were performed, in order to obtain an overall classification accuracy for the condition in that ROI.


  

