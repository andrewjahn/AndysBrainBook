.. _CONN_09_2ndLevel_Analysis:

================================
Chapter #9: Group-level analysis
================================

--------------------

Overview
********

Our goal in analyzing this dataset is to generalize the results to the population that the sample was drawn from. In other words, if we see changes in functional connectivity in our sample, can we say that these changes would likely be seen in the population as well?

To test this, we will run a **group-level (or 2nd-level) analysis**: we calculate the standard error and the mean for a correlation estimate, and then test whether the average estimate is statistically significant.


Setting up the Group-Level Analysis
***********************************

In the last chapter, we analyzed a single run for a single subject - in other words, we ran a 1st-level analysis. This is appropriate for determining whether there are significant correlations within a single subject, but we cannot do any group inference. To load more subjects, we will first have to download more subjects. Go back to the Arithmetic study (insert link), and download the functional and anatomical data for subjects 2-6.

.. note::

  If your Downloads folder is the default, you can move all of the downloaded images to their respective folders by typing:
  
  ::
  
    movefile('~/Downloads/*T1w*', 'anat')
    movefile('~/Downloads/*task-rest_bold*', 'func')
 

From the CONN GUI, click on the Setup tab and click ``Basic``. Change the number of subjects from 1 to 6, and observe how the number of sessions and the repetition time expands to reflect the total number of participants. Each number corresponds to each subject; for example, the third number in the "Number of sessions or runs" field corresponds to the third subject that we will load. If the parameters are different for any of the subjects, make sure that the edits match the subject.

.. figure:: 09_Group_Basic.png

We will then load each subject's anatomical and functional data. The easiest way to do this is to use the ``Find`` filter on the right-hand side of the CONN interface. In the field below the image extensions (i.e., *.img, *.nii, etc.), type "T1w" and then click ``Find``. The current directory and its sub-directories will be searched for any images that contain that string.

.. figure:: 09_SelectAnatomicals.png

Hold shift and then click to select all of the Subjects in the ``Subjects`` field, and then hold shift and click to select all of the "T1w.nii" images that are in the image selection window. Then click the ``Import`` button. You should see something like the following:

.. figure:: 09_AnatomicalsSelected.png

.. note::

  As before, scroll through the slices of your anatomical images to make sure that there are no artifacts, and that the images are oriented correctly.
  
Now do the same procedure for the functional images. Click on the ``Functional`` tab, and use the ``Find`` filter to look for any images in your directories that contain the string "bold". Use shift and click to highlight the Subjects and the resting-state images, and then click ``Import``. After a few moments, you should see a pop-up window saying that "6 files have been assigned to 6 subjects".

.. note::

  As you look through the functional images for these six subjects, do you notice any anomalies? (Hint: There is one in subject 2.) Do you think this will be a problem? If so, which preprocessing step should you pay particularly close attention to?

If you have already analyzed subject 1, you may notice that the data in the ``ROIs`` and ``Covariates`` tabs are the same, regardless of which subject you select. As we preprocess the subjects, these fields will be filled in with the ROIs and covariates for each subject.
  
Preprocesing the Subjects
^^^^^^^^^^^^^^^^^^^^^^^^^

Click on the ``Preprocessing`` button to begin preprocessing all of the subjets in a single batch. This will take about 5-6 minutes per subject, or around 30-40 minutes total.
