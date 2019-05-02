.. _04_Stats_General_Linear_Model.rst

Chapter 4: The General Linear Model
==================

.. note::

  This chapter is a brief introduction to the General Linear Model (GLM), and how it is applied to fMRI data. For a more complete introduction to the GLM, see `this tutorial <http://www.statsoft.com/Textbook/General-Linear-Models>`__ by StatSoft.

---------
 
Introduction
**********


Now we come to the **General Linear Model**, or GLM. With a GLM, we can use one or more **regressors**, or independent variables, to predict some outcome measure, or **dependent variable**. We compute numbers called **beta weights**, which are the relative weights assigned to each regressor to best fit the data. Any discrepancies between the model and the data are called **residuals**.

The symbols representing each of these terms are shown in the following equation, which can be shortened or expanded depending on the number of regressors in your model:

.. figure:: GLM_Equation.png

Let's see how to apply this to a simple example. Imagine that we want to predict GPA based on height, IQ, and number of drinks per week. We may find that IQ has a positive association with GPA, number of drinks has a negative association, and height has no association at all; and we assign each of these regressors beta weights to best fit the data. For example, maybe each additional IQ point is associated with an additional 0.05 increase in GPA, while each additional drink per week is associated with a -0.07 decrease in GPA. In that case our model and its beta weights would look something like this (in which asterisks represent beta weights that are significant):

.. figure:: GLM_Example.png


This GLM can be expanded to include many regressors, but however many there are, the GLM assumes that the data can be modeled as a linear combination of each of the regressors - hence the name General Linear Model. We will see how to apply the GLM to fMRI data in the next chapter.


Applying the GLM to fMRI data
**********

Conceptually, we’re doing the same thing when we use several regressors to estimate brain activity, which is our dependent variable with fMRI data. Specifically, we estimate the average amplitude of the BOLD signal in response to each condition in our model. 

In the animation below, the different colors of the BOLD responses indicate different conditions, and the gray line represents the timecourse of our preprocessed data. This shows how the amplitude of each condition is being estimated to best fit the data; for the condition on the left, it is relatively high, whereas for the condition on the right, it is relatively low. You can also imagine a condition's BOLD signal which is not significantly different from zero, or which is even negative.

.. figure:: GLM_fMRI_Data.gif

The result is a **fitted time series**, shown in the animation as a blue line. What we have done here is fitted a model to the time series, which is explained in more detail in the next chapter.

.. note::

  We do the procedure above for every voxel in the brain, since every voxel has its own timecourse. This approach is known as a **mass univariate** approach, since we estimate these beta weights at each voxel in the brain. As there are tens or hundreds of thousands of voxels in a typical fMRI dataset, later we will need to correct for all of the tests we have done. This will be covered in a later chapter on group analysis.
