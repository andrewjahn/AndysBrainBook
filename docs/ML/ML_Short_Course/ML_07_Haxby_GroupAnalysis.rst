.. _ML_07_Haxby_GroupAnalysis:

============================================
Machine Learning Tutorial #7: Group Analysis
============================================

---------------

Overview
********

As in our tutorials on fMRI analysis, our goal in analyzing this dataset is to generalize the results to the population that the sample was drawn from. In other words, if we see a significant classification accuracy in these subjects, can we say that it would likely be seen in the population as well?

With MVPA, we have two different types of group analysis we can do: One with the ROI results, and one with the searchlight results. We will review how to do each of these in turn.


Group Analysis: ROI
*******************

One of the first-level analyses we did was a classification analysis within a region of interest, or ROI. In this case, we used the masks created in the original Haxby study, and generated a single classification accuracy for each condition for each subject. When we scripted our analyses in the last chapter, these results were stored in the directory ``ROI_Results``; in order to do a group analysis, we will need to extract the classification accuracy for the conditions we are interested in, and then compare the accuracy across conditions.

.. Future chapter topic: Comparing these results to a control ROI in another region, or outside the brain

The code below will extract the classification accuracy and store them in variables. For example, the cell ``(4,4)`` indexes the classification accuracy for "Faces", since that was the fourth condition that we entered in our list of labelnames for the classifier:

::

  %%% Define the variables

  subjects = [1 2 3 4 5 6];
  Group_Results_Face = [];
  Group_Results_House = [];
  Group_Results_Scissors = [];

  %%% Extract confusion matrix accuracy for Faces, Houses, and Scissors

  for subject=subjects

      subject = num2str(subject);

      ROI_Results = load([pwd '/SPM_Results_' subject '/ROI_Results/res_confusion_matrix.mat']);

      Group_Results_Face = [Group_Results_Face, ROI_Results.results.confusion_matrix.output{1}(4,4)];
      Group_Results_House = [Group_Results_House, ROI_Results.results.confusion_matrix.output{1}(5,5)];
      Group_Results_Scissors = [Group_Results_Scissors, ROI_Results.results.confusion_matrix.output{1}(6,6)];

  end



We can then compare the classification accuracies for Faces against that of Houses, for example; or that of Faces versus Scissors:
::

  %%% Example t-tests %%%
  [F_H_h, F_H_p, F_H_ci, F_H_stats] = ttest(Group_Results_Face, Group_Results_House);
  [F_S_h, F_S_p, F_S_ci, F_S_stats] = ttest(Group_Results_Face, Group_Results_Scissors);
  
In the first case (Faces compared to Houses), the average classification accuracy within this ROI is virtually identical - the resulting t-test is not significant. The comparison of Faces against Scissors, on the other hand, yields a t-statistic of 5.80, p<0.001, representing a significant difference between the two.


Group Analysis: Searchlight
***************************

The group analysis of the searchlight results requires more steps. Remember that our preprocessing pipeline did not include normalization, since we wanted to introduce as few interpolations as possible; this also enabled us to use subject-specific masks for our ROI group analysis.

Our searchlight maps, located within the ``SPM_Results`` directories, will instead need to be normalized before they can be used in a group analysis. To do this, open SPM, and then click ``Segment``. In the ``Volumes`` field, select the image ``rsub-1_T1w.nii`` from the directory ``sub-1/anat``, and in the ``Save Bias Corrected``, change the selection from ``None`` to ``Save Bias Corrected``. At the very bottom of the window, change ``Deformation Fields`` to ``Forward``, and then click the Green Go button. This will only take a minute or two.

When it has finished, go back to the SPM GUI and select ``Normalise -> Write``. Select the file ``y_rsub-1_T1.nii`` from the subject's ``anat`` directory, and for ``Images to Write``, select the realigned functional images within the ``func`` directory (you can find them easily by navigating to the func directory, entering ``^r`` in the Filter, and setting the Frames to ``1:300``. Click ``Done``, and then set the Voxel sizes to ``[3 3 3]``. (You may choose a different normalized resolution; this is simply done to save time for the tutorial and save space on your hard drive.) Then click the Green Go button.
