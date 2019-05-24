.. _fMRI_09_ROIAnalysis:

fMRI Tutorial #9: ROI Analysis
=====================

---------

Overview
********

You've just completed a group-level analysis, and identified which regions of the brain show a significant difference between the Incongruent and Congruent conditions of the experiment. For some researchers, this may be all that they want to do.

This kind of analysis is called a **whole-brain** or **exploratory** analysis. These types of analyses are useful when the experimenter doesn't have a hypothesis about where the difference may be located; the result will be used as the basis for future research.

When a large number of studies have been run about a specific topic, however, we can begin to make more specific hypotheses about where we should find our results in the brain images. For example, cognitive control has been studied for many years, and many fMRI studies have been published about it using different paradigms that compare more cognitively demanding tasks to less cognitively demanding tasks. Often, significant changes in the BOLD signal are seen in a region of the brain known as the **dorsal medial prefrontal cortex**, or dmPFC for short. For the Flanker study, then, we could restrict our analysis to this region and only extract data from voxels within that region. This is known as a **region of interest** analysis, or ROI analysis for short. A general name for an analysis in which you choose to analyze a region selected before you analyze the results is called a **confirmatory analysis**.

The whole-brain maps that we generate can hide important details about the effects that we’re studying. We may find a significant effect of incongruent-congruent, but the reason the effect is significant could be because incongruent is greater than congruent, or because congruent is much more negative than congruent, or some combination of the two. The only way to determine what is driving the effect is by ROI analysis, and this is especially important when dealing with interactions and more sophisticated designs.


Using Atlases
*******

One way to create a region for our ROI analysis is to use an **atlas**, or a map of the brain that partitions it into anatomically distinct sections. 
