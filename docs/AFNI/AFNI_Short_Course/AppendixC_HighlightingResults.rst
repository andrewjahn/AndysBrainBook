.. _AppendixC_HighlightingResults:

===========================================
Appendix C: Highlighting Vs. Hiding Results
===========================================


Overview
--------

In the past few years, reproducibility has become a more prominent issue in the social sciences, as well as within cognitive neuroscience. For example, as far back as 2005, John Ioannidis published a paper on why most published research findings were probably false, using simulations of variables such as bias and pre-study odds. Although the paper dealt with hypothetical scenarios, these scenarios were demonstrated to have played out across many published findings in the social sciences: Brian Nosek and colleagues, in a paper published in *Science* in 2015, attempted to replicate the effects of 100 studies, were able to replicate only 39% of the effects, as determined by whether they passed the conventional p=0.05 threshold. This brought into question whether strategies such as p-hacking and selective reporting were leading to inflated effect sizes and spurious results.

Since then, many studies have also been carried out in cognitive neuroscience to find out whether effects are generally reproducible or not, and if not, what practices may hindering reproducibility. For example, a study by Botvinick-Nezer et al., 2020, provided data to several dozens teams of independent researchers, and asked them to test for given effects, such as whether there was a significant effect of viewing the potential Gain of a gamble compared to its potential loss; this included whole-brain analyses which determined whether there were significant effects in *a priori* regions of interest (ROIs), such as the ventral striatum, amygdala, and ventromedial prefrontal cortex - all regions that typically show BOLD activity in response to gains and losses.

Once the analyses were completed by each team, they were then asked to make a decision as to whether the effect existed in a given region or not; in which case, the team could either reject the null hypothesis and say that there was probably an effect in that region, or fail to reject the null hypothesis. In the final paper, around 80% of the teams reported a significant effect for one of the findings, with most of the other hypotheses (of which there were nine total) having teams report around 20%-40% significant effects; in addition, a figure of Spearman correlation values of each team's illustrated how much overlap there was in the unthresholded maps between each group.

Benefits of the Highlighting Approach
-------------------------------------

At first glance, it may look as though there was less overlap of significant effects between the teams than might be expected, given that the effects of Gain and Loss in regions such as the ventromedial prefrontal cortex and ventral striatum are consistently reported in the literature. Given this incongruency, two hypotheses come to mind: Either the reported effects in the literature are in fact either spurious or inflated due to the practices listed above, or many of the teams in the Botvinick-Nezer study were not analyzing the data correctly. Neither hypothesis is encouraging for those who wish to see results easily reproducible by independent researchers.

However, in a recent paper by Taylor et al. (2023), there is a third possibility: It may be that the vast majority of researchers are in fact doing the right analyses, and that the effects across independent teams are more similar than we think. It could be that it is not their fault that the effects are not significant, but rather the reliance on the conventional p=0.05 threshold for determining statistical significance. If we make the main focus on the effect sizes instead of statistical significance, we may see that there is less of a reproducibility crisis than we think.

Consider, for example, how most results are displayed. After preprocessing the data and analyzing it using a general linear model, the maps are thresholded, using any of a range of values, but most commonly p=0.05, whether it is voxel-wise corrected or cluster-wise corrected.
