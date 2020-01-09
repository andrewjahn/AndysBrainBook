.. _SPM_08_GroupAnalysis:

===============================
SPM Tutorial #8: Group Analysis
===============================

--------

Overview
***************

Our goal in analyzing this dataset is to generalize the results to the population that the sample was drawn from. In other words, if we see changes in brain activity in our sample, can we say that these changes would likely be seen in the population as well?

To test this, we will run a **group-level analysis** (also known as a **second-level analysis**). In SPM, this means that we calculate the standard error and the mean for a contrast estimate, and then test whether the average estimate is statistically significant. We will be doing this group-level analysis using a **summary statistic** approach which ignores the variability in the parameter estimates, and performs a t-test over the mean parameter estimates from each subject.


Specifying the 2nd-Level Analysis
*********************************





Video
*****
