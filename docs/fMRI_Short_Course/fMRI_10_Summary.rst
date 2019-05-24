.. _fMRI_10_Summary:

fMRI Tutorial #10: Summary
==================

We have reached the end of our fMRI course. Now is a good time to take a break and relax. This may be for a few hours or a few weeks; but when you feel ready, come back and think about what you have learned.

Basic fMRI Concepts
*********

The dataset we analyzed is a relatively simple one: It has two conditions, and we already had a good idea of where we would find a significant result. 

Nevertheless, you learned many important concepts that are applicable to any fMRI study. Here is a list of a few of them:

1. fMRI images, or **volumes**, are collected and strung together like beads on a string to create **runs** of functional data. Each of these volumes is a three-dimensional image that is composed of small three-dimensional cubes called **voxels**. The signal measured in each voxel over the entire run is called a **time-series**, which is an indirect measure of neural activity as indexed by the **BOLD signal**.

2. Just like digital images that we take with a phone or a camera, we clean up our fMRI images with **preprocessing**. The basic preprocessing steps include slice-timing correction, motion correction, smoothing, and temporal filtering. These were all covered in the chapter on :ref:`preprocessing <fMRI_04_Preprocessing>`.

3. After cleaning up the images, we then **fit a model** to the timeseries. This meant creating a model, or **ideal time-series**, of what we thought the BOLD signal should look like in a voxel, given our **onset times** indicating which event happened at which time. We then convolved these onset times with a mathematical function called the **Hemodynamic Response Function**, a model of how we believe the BOLD signal in the brain rises and falls in response to a stimulus. All of this, and more, was discussed in the chapter on :ref:`statistical modeling <fMRI_05_1stLevelAnalysis>`.

4. We then created a script to automate the analysis of all of our subjects. This required learning quite a bit of Unix code, but the effort is worth it: Learning just a little about scripting can save you countless hours of drudgery.

5. Once every subject was preprocessed and had a statistical model fit to their timeseries, we were able to run 2nd-level and 3rd-level analyses.

6. Once the higher-level analyses were complete, we had two options: 1) Run an **exploratory analysis** in which we analyze each model at every voxel in the brain, and view the resulting whole-brain maps; or 2) Do an **ROI analysis** in which we focused on a subset of voxels within the brain, extracting parameter estimates for each subject and then running a statistical analysis on those numbers. Both types of analysis can be used in a single study, to show both the extent of the whole-brain results, and to focus on more targeted confirmatory analyses.

Take some time to review each of these steps, and maybe even run the entire analysis again from scratch - challenging yourself by seeing if you can do it from memory. I recommend either writing down or saying out loud why you are doing each step when you do it, to reinforce the concepts behind each part of the analysis. When you begin to feel more confident about analyzing fMRI data on your own, try the following exercises. The biggest and most rewarding challenge, of course, will be applying these concepts to your own fMRI data.

Good luck!


Exercises
********
