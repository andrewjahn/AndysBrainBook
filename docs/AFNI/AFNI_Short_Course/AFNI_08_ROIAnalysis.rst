.. _AFNI_08_ROIAnalysis:

==============================
fMRI Tutorial #8: ROI Analysis
==============================

---------

Overview
********

You've just completed a group-level analysis, and identified which regions of the brain show a significant difference between the Incongruent and Congruent conditions of the experiment. For some researchers, this may be all that they want to do.

This kind of analysis is called a **whole-brain** or **exploratory** analysis. These types of analyses are useful when the experimenter doesn't have a hypothesis about where the difference may be located; the result will be used as the basis for future research.

When a large number of studies have been run about a specific topic, however, we can begin to make more specific hypotheses about where we should find our results in the brain images. For example, cognitive control has been studied for many years, and many fMRI studies have been published about it using different paradigms that compare more cognitively demanding tasks to less cognitively demanding tasks. Often, significant increases in the BOLD signal during cognitively demanding conditions are seen in a region of the brain known as the **dorsal medial prefrontal cortex**, or dmPFC for short. For the Flanker study, then, we could restrict our analysis to this region and only extract data from voxels within that region. This is known as a **region of interest (ROI)** analysis. A general name for an analysis in which you choose to analyze a region selected before you look at whole-brain results is called a **confirmatory analysis**.

Whole-brain maps can hide important details about the effects that we’re studying. We may find a significant effect of incongruent-congruent, but the reason the effect is significant could be because incongruent is greater than congruent, or because congruent is much more negative than congruent, or some combination of the two. The only way to determine what is driving the effect is with ROI analysis, and this is especially important when dealing with interactions and more sophisticated designs.


Using Atlases
*******

One way to create a region for our ROI analysis is to use an **atlas**, or a map that partitions the brain into anatomically distinct regions.

AFNI comes with several atlases in both Talairach and MNI space, which can be accessed through the AFNI GUI. Finding the atlases can be difficult - you must first click on ``Define Datamode``, and then click on ``Plugins``, and from the dropdown menu select ``Draw Dataset``. The figure below shows the Draw Dataset window.

.. figure:: 08_DrawDataSetWindow.png

  After opening up the AFNI GUI, click on the buttons in the order shown in the figure (1, 2, and 3).
  
Once you have opened the Draw Dataset window, you will first need to click on the button ``Choose dataset for copying``. Since all of our data has been normalized to the MNI_avg152T1 template, we have two options:

1. Select the MNI_avg152T1+tlrc template from the ``abin`` directory; or
2. Select one of the normalized anatomical images from the Flanker dataset.

In both cases, the images will be in MNI space and will have the same dimensions and voxel resolution. The purpose of making a copy of that dataset is to create a "clean" dataset with the same dimensions as the other images, but which we can write on by marking whichever voxels we want to belong to our ROI. In this case, navigate to the ``sub-01/sub-01.results`` directory, open the AFNI GUI, and open the Draw Dataset window. For the image to copy, select the file ``anat_file.sub-01``.

Once you have done that, you have several different atlases to select from. For the current tutorial, select the atlas ``DD_Desai_MPM``, and then click on the dropdown menu below it. You have many different regions to choose from, and the voxels represented by each label can be guessed at by the name; for example, ``ctx_lh_G_and_S_frontomargin`` probably refers to the cortical voxels of the gyrus and sulcus of the frontomarginal region of the left hemisphere.

Select ``ctx_lh_G_and_S_cingul_-Mid_Ant``, and then click on the button ``Load: InFill``. This will highlight in red all of the voxels belonging to that region of the atlas. You can undo this by clicking on the ``Undo`` button, which keeps several steps in memory. Now right-click the area to the left of the label dropdown menu to open a more compact view of the atlas regions, and select ``ctx_rh_G_and_S_cingul-Mid-Ant``. Click on ``Load: InFill`` to add that region to the current mask, and then click ``SaveAs``. Call the output ``midACC``. This will create a new file that contains values of "1" in the voxels that belong to the region, and zeros everywhere else; this is also known as a **mask**. When you are finished, click ``Done``.

.. figure:: 08_AtlasMask.png

  You can choose an atlas from the dropdown menu (1), and then choose a corresponding label from the atlas (2). Clicking on Load: InFill (3) will highlight those voxels within that label, and you can save the mask by clicking on SaveAs (4).


.. warning::

  Your results will have the same resolution as the template you used for normalization. The AFNI atlas we used is the MNI_avg152T1+tlrc file, which has a resolution of 3x3x3mm. When you create a mask, it will have the same resolution as the template that it is overlaid on. When we extract data from the mask, the data and the mask need to have the same resolution. To avoid any errors due to different image resolutions, either use one of your normalized anatomical images, or the template that you normalized to.
  

Extracting Data from an Anatomical Mask
************

Once you've created the mask, you can then extract each subject's contrast estimates from it. There are two ways that we could extract our contrast of interest Incongruent-Congurent:

1. Extract the contrast estimate Incongruent-Congruent from our stats file; or
2. Extract the individual beta weights for Incongruent and Congruent separately, and then take the difference between the two.

As we will see, option #2 allows you to determine what is driving the effect; in other words, whether a significant effect is due to both beta weights being positive but the Incongruent beta weights being more positive, both weights being negative but the Congruent betas more negative, or a combination of the two. It is only by extracting both sets of beta weights that we can determine this.

First, from the subjects directory type:

::

  3dinfo -verb sub-01/sub-01.results/stats.sub-01+tlrc.
  

This will return a list of all the beta weights and contrast weights contained in the stats file. 

.. figure:: 08_stats_weights.png

The sub-briks index which beta weight belongs to which volume in the dataset. In this example, the beta weight for the Congruent condition is sub-brik 1, the beta weight for the Incongruent condition is sub-brik 4, and the contrast weight for Incongruent-Congruent is sub-brik 7. For this tutorial, we will extract sub-briks 1 and 4 and store them in separate files, and then extract the values for each subject from an ROI.

The individual sub-briks can be extracted using the following code, `extractBetas.sh <https://github.com/andrewjahn/AFNI_Scripts/blob/master/extractBetas.sh>`__:

::

#!/bin/bash

for subj in `cat subjList.txt`; do

	3dbucket -aglueto Congruent_betas+tlrc.HEAD ${subj}/${subj}.results/stats.${subj}+tlrc'[1]'
	3dbucket -aglueto Incongruent_betas+tlrc.HEAD ${subj}/${subj}.results/stats.${subj}+tlrc'[4]'
  
done


When it finishes, you will have generated two new datasets: Congruent_betas and Incongruent_betas. Open up one of the datasets in your viewer, and click on the ``Graph`` button of the AFNI GUI to scroll through the different volumes. How is this "time-series" different from the time-series you viewed in the raw imaging data? As another exercise, from the command line type ``3dinfo -nt Congruent_betas+tlrc``, in which the "-nt" option returns the number of volumes (or time-points) in the dataset. What number is returned, and what does it represent? Does it make sense?

.. note::

  Each number output from this command corresponds to the contrast estimate that went into the analysis. For example, the first number corresponds to the average contrast estimate for Incongruent-Congruent for sub-01, the second number is the average contrast estimate for sub-02, and so on. These numbers can be copied and pasted into a statistical software package of your choice (such as R), and then you can run a t-test on them.
  
Extracting Data from an Sphere
************

You may have noticed that the results from the ROI analysis using the anatomical mask were not significant. This may be because the ACC mask covers a very large region; although the ACC is labeled as a single anatomical region, we may be extracting data from several distinct functional regions. Consequently, this may not be the best ROI approach to take.

Another technique is called the **spherical ROI** approach. In this case, a sphere of a given diameter is centered at a triplet of specified x-, y-, and z-coordinates. These coordinates are often based on the peak activation of another study that uses the same or a similar experimental design to what you are using. This is considered an **independent** analysis, since the ROI is defined based on a separate study.

The following animation shows the difference between anatomical and spherical ROIs:

.. figure:: 08_ROI_Analysis_Anatomical_Spherical.gif

To create this ROI, we will need to find peak coordinates from another study; let's randomly pick a paper, such as Jahn et al., 2016. In the Results section, we find that there is a Conflict effect for a Stroop task - a distinct but related experimental design also intended to tap into cognitive control - with a peak t-statistic at MNI coordinates 0, 22, 40.

.. figure:: 08_ROI_Analysis_Jahn_Study.png

The next few steps are complicated, so pay close attention to each one:

1. Open fsleyes, and load an MNI template. In the fields under the label "Coordinates: MNI152" in the ``Location`` window, type ``0 20 44``. Just to the right of those fields, note the corresponding change in the numbers in the fields under ``Voxel location``. In this case, they are ``45 73 58``. Write down these numbers.

2. In the terminal, navigate to the Flanker directory and type the following:

::

  fslmaths $FSLDIR/data/standard/MNI152_T1_2mm.nii.gz -mul 0 -add 1 -roi 45 1 73 1 58 1 0 1 Jahn_ROI_dmPFC_0_20_44.nii.gz -odt float

This is a long, dense command, but for now just note where we have inserted the numbers 45, 73, and 58. When you create another spherical ROI based on different coordinates, these are the only numbers you will change. (When you create a new ROI you should change the label of the output file as well.) The output of this command is a single voxel marking the center of the coordinates specified above.

3. Next, type:

::

  fslmaths Jahn_ROI_dmPFC_0_20_44.nii.gz -kernel sphere 5 -fmean Jahn_Sphere_dmPFC_0_20_44.nii.gz -odt float

This expands the single voxel into a sphere with a radius of 5mm, and calls the output "Jahn_Sphere.nii.gz". If you wanted to change the size of the sphere to 10mm, for example, you would change this section of code to ``-kernel sphere 10``.

4. Now, type:

::

  fslmaths Jahn_Sphere_dmPFC_0_20_44.nii.gz -bin Jahn_Sphere_bin_dmPFC_0_20_44.nii.gz
  
This will binarize the sphere, so that it can be read by the FSL commands.

.. note::

  In the steps that were just listed, notice how the output from each command is used as input to the next command. You will change this for your own ROI, if you decide to create one.

5. Lastly, we will extract data from this ROI by typing:

::

  fslmeants -i allZstats.nii.gz -m Jahn_Sphere_bin_dmPFC_0_20_44.nii.gz 
  

The numbers you get from this analysis should look much different from the ones you created using the anatomical mask. Copy and paste these commands into the statistical software package of your choice, and run a one-sample t-test on them. Are they significant? How would you describe them if you had to write up these results in a manuscript?


-------

Exercises
********

1. The mask used with fslmeants is **binarized**, meaning that any voxel containing a numerical value greater than zero will be converted to a "1", and then data will be extracted only from those voxels labeled with a "1". You will recall that the mask created with fsleyes is **probabilistic**. If you want to weight the extracted contrast estimates by the probability weight, you can do this by using the ``-w`` option with fslmeants. Try typing:

::

  fslmeants -i allZstats.nii.gz -m PCG.nii.gz -w
  
And observe how the numbers are different from the previous method that used a binarized mask. Is the difference small? Large? Is it what you would expect?

2. Use the code given in the section on spherical ROI analysis to create a sphere with a 7mm radius located at MNI coordinates 36, -2, 48.

3. Use the Harvard-Oxford subcortical atlas to create an anatomical mask of the right amygdala. Label it whatever you want. Then, extract the z-statistics from cope1 (i.e., the contrast estimates for Incongruent compared to baseline).

--------

Video
*********

Click `here <https://www.youtube.com/watch?v=p70utwa-NkU>`__ for a demonstration of how to use both anatomical and spherical masks for an ROI analysis.
