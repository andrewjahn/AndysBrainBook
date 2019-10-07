.. _AFNI_07_GroupAnalysis:

=====================
AFNI Tutorial #7: Group Analysis
=====================

--------

Overview
***************

Our goal in analyzing this dataset is to generalize the results to the population that the sample was drawn from. In other words, if we see changes in brain activity in our sample, can we say that these changes would likely be seen in the population as well?

To test this, we will run a **group-level analysis** (also known as a **second-level analysis**). In AFNI, this means that we calculate the standard error and the mean for a contrast estimate, and then test whether the average estimate is statistically significant.
