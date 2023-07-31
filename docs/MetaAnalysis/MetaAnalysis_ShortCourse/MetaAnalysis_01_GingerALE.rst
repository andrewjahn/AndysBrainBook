.. _MetaAnalysis_01_GingerALE:

==========================================================================
Meta-Analysis Tutorial #1: Activation Likelihood Estimation with GingerALE
==========================================================================

---------------

Overview
********

The earliest meta-analysis tool developed for neuroimaging data was a method called **Activation Likelihood Estimation**, or ALE. First demonstrated by Turkeltaub et al. in 2002, ALE estimates the likelihood of overlap between the activation of different studies (also referred to as the "concordance" between the studies) as compared to a null distribution computed over several permutations. In this way, it is similar to :ref:`how significant clusters are calculated for fMRI studies <Appendix_A_ClusterCorrection>`: The cluster reported in your study is compared to a distribution of clusters generated at random, and the cluster is deemed significant if it is larger than the largest 5% of randomly generated clusters.

.. figure:: GingerALE_01_Turkeltaub.png

  Figures 2 and 3 from Turkeltaub et al., 2002. Figure 2 illustrates an example of the Activation Likelihood Estimate derived from the meta-analysis data (blue line) compared to a random noise distribution (red line). The black line bisecting the two lines represents an alpha threshold of p=0.0001 for the null distribution, and the data distribution is compared to the null distribution at that level; in this case, all of the ALE values for the dataset were greater than the null dataset at the p=0.0001 level. Figure 3 depicts the thresholded results of the meta-analysis side-by-side with the results of a separate fMRI study to illustrate their similarities and to validate the method.

Further modifications to Turkeltaub's original ALE study led to a new meta-analysis package called `GingerALE <https://www.brainmap.org/ale/>`__. This is part of a suite of packages called BrainMap, which also includes the auxiliary programs Scribe and Sleuth; for now, however, we will just focus on GingerALE, demonstrating how to replicate the results of another meta-analysis, and then to contrast two meta-analyses to determine where there is either overlap or differences between them. And to do that, we first need to briefly review the GingerALE Interface.


The GingerALE Interface
***********************

The GingerALE package can be downloaded by clicking on `this link <https://www.brainmap.org/ale/>`__ and choosing the version matching your operating system; downloading and installing the package is straightforward, and should only take a couple of minutes. Click on the file once you have downloaded it, and then click and drag the GingerALE icon into your Applications directory, which allows you to open the package by using your Finder to search for "GingerALE". Click on the software package result that appears, which will open the GingerALE interface.

.. figure:: GingerALE_02_Interface.png

The radio button by default is set to "Single Dataset", which is what we will start with; that is, we will test whether the results of several different studies examining the same contrast have significant overlap. Not all of the studies need to use the exact same contrast, it should be noted; there will be differences in experimental design, as well as number of subjects and number of trials. But each study that is included in the meta-analysis ought to tap into the same underlying cognitive phenomenon, whether it be working memory, cognitive control, and so forth. In the Turkeltaub et al. 2002 paper discussed above, for example, they included studies which measured where reading aloud elicited higher PET signals in the brain, relative to some control condition. Here is a partial list of those controls:

* Silent Reading
* Fixation
* Rest

As you can see, there were a variety of control conditions, which entails more variability when comparing studies; but since all of the contrasts used Reading, we can combine them into a meta-analysis. It is up to you to define the criteria for which studies you will include in your meta-analysis.

You may notice that the default coordinate system listed by GingerALE is "Talairach", a template used by many neuroimaging studies in the 1990s and early 2000s, but which has been superseded by a newer template space, Montreal Neurological Institute (or MNI). To change the coordinate space to MNI, click on ``GingerALE -> Settings``, and select the radio button next to "MNI152" - a variation of the MNI template composed of an average of 152 subjects, and with slightly different dimensions than MNI.

The next field, "Mask Size", specifies whether to make the mask bigger or smaller - that is, to make the space of our analysis included more or fewer voxels. It may be the case, for example, that one of the studies included in your meta-analysis used a slightly bigger mask than usual, and that this in turn led to a peak activation being located beyond the edge of a more conservative mask used by the other studies. Later on you may find that the foci you want to use in your study fall outside the default, more conservative mask used by GingerALE, and you can change that here.

All of the other options are mostly self-explanatory, and the defaults should be left as they are. The only other option you may want to change is the "Output Directory" field, which by default points to the home directory. To better organize our results, create a new directory on your Desktop called ``GingerALE_Demo``, and then select this folder as your output directory. When you are finished, your GingerALE preferences should look like this:

.. figure:: GingerALE_03_Preferences.png



Replicating the Meta-Analysis of Kumar et al., 2016
***************************************************
