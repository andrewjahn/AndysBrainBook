.. _Appendix_A_ClusterCorrection:

Appendix A: Cluster Correction
==============

-------------

Overview
*************

A mass univariate analysis conducts as many statistical tests as there are voxels. Given that a typical fMRI dataset contains tens of thousands of voxels, this quickly leads to a large number of false positives. To control for the number of false positives, therefore, and to keep them at the conventional level of 5%, we will need to do something called **multiple comparisons correction**.

.. note::

  For most statistical tests, we set a **false positive rate**, also known as the **alpha level**, to 0.05, or 5%. This is the probability that we will reject the null hypothesis if the null hypothesis is true; in other words, the probability that we will observe a false positive.
  
  This approach is known as **Null Hypothesis Significance Testing**, and a full treatment of it is outside the scope of this book. There are many introductory textbooks and online tutorials which you can find easily; I don't have any recommendations about which one to use, and I plan to make my own introduction sometime soon.

To build your intution about what this means, imagine the following scenario:

[Think of an entertaining yet instructive example]


As you can see, the more tests that we do, the more likely we are to find a significant effect by chance alone - in other words, there isn't actually an effect, but we claim that there is one.

In the FEAT GUI ``Post-stats`` tab, the Thresholding dropdown menu has different multiple comparisons correction options. The first one, ``None``, signifies that no correction or thresholding will be done; all of the voxels' statistics will be shown, regardless of whether they are significant or not. The second, ``Uncorrected``, will threshold each voxel individually at a specified p-level, but won't correct for the number of tests that are conducted. ``Voxel`` will perform **Bonferroni** correction, which divides the alpha level by the total number of tests - that is, the number of voxels in your dataset. In other words, if you have 100,000 voxels, and you wish to keep an alpha level of 0.05, an individual voxel will have to pass a significance threshold of 0.05/100,000 = 0.0000005 in order to be deemed significant.

Problems with Bonferroni Correction
*************

Although Bonferroni correction will ensure that your false positive rate is no higher than 5%, it is also a very conservative test. Bonferroni correction is valid only when each test is independent - to take our fMRI dataset as an example, that would mean that each voxel is completely independent of every other voxel in the brain; knowing the value of one voxel doesn't give you any information about any other voxel.

