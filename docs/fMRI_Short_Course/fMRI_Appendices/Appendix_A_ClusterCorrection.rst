.. _Appendix_A_ClusterCorrection:

Appendix A: Cluster Correction
==============

-------------

Overview
*************

A mass univariate analysis conducts as many statistical tests as there are voxels. Given that a typical fMRI dataset contains tens of thousands of voxels, this quickly leads to a large number of false positives. To control for the number of false positives, therefore, and to keep them at the conventional level of 5%, we will need to do something called **multiple comparisons correction**.

To build your intution about what this means, think of the following scenario:
