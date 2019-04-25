.. _Checkpoint.rst


Checkpoint: What have we done so far?
==========

Now is a good opportunity to review what we have done so far:

1. We have downloaded an fMRI dataset that has anatomical and functional images;

2. We looked at the data in FSLeyes, FSL's data viewer;

3. We preprocessed the data using FEAT, FSL's preprocessing tool.


Along the way you learned how to check the images before and after each preprocessing step. As you apply the same quality checks to different datasets, you will come across images that are difficult to judge - they may seem to be on the border of being acceptable or unacceptable.

It may be confusing at first. But over time you will develop your judgment about what images are clearly good, which ones are obviously bad, and which ones you will need to think carefully about either keeping or discarding. The more you think about why the results of a preprocessing step look good or bad, the easier it will become to make your judgments quicker and more accurately.


Try the following exercises to become more fluent with FSL and with judging the output from each preprocessing step.

-----------

Exercises
********

1. Run BET on the anatomical image sub-08_T1w.nii.gz with gractional intensity thresholds of 0.1 and 0.9. Take a snapshopt of each output image with FSLeyes using the camera button (it is located in the upper middle part of the viewer). Note the differences between the two. Is the output what you expected? If you had to use one image or the other, which one would you choose?

2. Preprocess run 2 of the functional data using the FEAT GUI. To do this, select ``sub-08_task-flanker_run2.nii.gz`` from the ``func`` directory, change the output directory to ``run2``, and make sure ``Preprocessing`` is selected from the dropdown menu. Keep the other settings the same as when you analyzed run 1.

3. Preprocess run 1 using a 3mm smoothing kernel, keeping the other preprocessing options the same. (Make sure, however, to change the output directory to a new name in order to keep the output organized.) What does the output ``filtered_func_data.nii.gz`` look like?

4. What do you see in the Registration tab of the FEAT output when you preprocess run 1 using ``3DOF`` for registration and normalization? How is it different from what you saw when you ran the preprocessing with ```12DOF``? Why?

5. Rerun registration for run 1 using ``BBR`` instead of ``12DOF``. What difference does it make?
