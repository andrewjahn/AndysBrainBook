.. _06_Stats_Running_1stLevel_Analysis.rst

Chapter 6: Running the First-Level Analysis
================

---------

Setting up the Model in FEAT
***************

Navigate to the ``sub-08`` directory, and type ``fsl`` from the command line. Open the FEAT GUI, and from the dropdown menu in the upper right of the Data tab, change "Full Analysis" to "Statistics". This will grey out the Pre-stats and Registration tabs, since we have already done them. You will also see a new button called "Input is a FEAT directory". Check the box, and select the FEAT directory ``run1.feat`` that has already been preprocessed. Click OK and ignore the warning about loading design information from the design.fsf file.

Next, click on the ``Stats`` tab. As with many of the FEAT tabs, there are many options, but we will only focus on a couple of them. Click on "Full model setup", and set the Number of original EVs to 2. This will create two tabs, one for each regressor. In the "1" tab's EV name field, type "incongruent". Click on the dropdown menu next ot Basic shape, and select "Custom (3 column format"). This reveals a field called "Filename"; click on the folder icon to select the timing file "incongruent_run1.txt". Uncheck the "Add temporal derivative" button. Click on the "2" tab and do the same thing, only this time selecting the timing file "congruent_run1.txt".

When you have finished that, click on the ``Contrasts & F-tests`` tab. Set the number of contrasts to 3, and type the following contrast names in each row, along with the following **contrast weights** in EV1 and EV2:

1. incongruent [1 0];
2. congruent [0 1];
3. incongruent-congruent [1 -1].

Click on OK, which will open up a **Design Matrix** window. This shows both the ideal time-series, with the time of the run represented as the top of the column and the end as the bottom of the column, and the contrast weights. Note how the ideal time-series corresponds to the timing file that you just created. Then, click Go to run the model.


What does the ideal time-series represent?
***************


To put this in mathematical terms, each voxel has a time series, which we’ll notate as Y. We also have our two regressors, which we’ll notate as x1 and x2. These regressors constitute our design matrix, which we’ll notate with a large X. So far, all of these variables are known - Y is measured from the data, and x1 and x2 are constructed from convolving the HRF and the timing onsets. Now, since the next part gets into matrix algebra, I’m going to switch the orientation of these timecourses: Normally we think of think of them as running from left to right, but now we’re going to depict them as going from top to bottom. 

The next part of this equation is the beta weights, which we’ll notate as B1 and B2, corresponding to the x1 and x2 regressors. These represent the amount that the HRF needs to be scaled to best match the original data in Y, and these weights are estimated - hence the name “beta weights”. The last term in this equation is E, which represents the residuals, or the difference between our ideal time series model and the data after estimating the beta weights.

.. figure:: GLM_fMRI_Data.gif


.. Examining the output


