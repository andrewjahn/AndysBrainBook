.. _FIR_Overview:

=============================
Finite Impulse Response (FIR)
=============================

----------------

Overview
********

If you have completed the tutorials on task-based fMRI studies, you are familiar with how **basis functions** are convolved with onset times to best fit the time-series. One drawback of this method is that a single best-fitting parameter is estimated for all of the instances of a particular condition; what if instead we wanted to examine the average time-course of the activity after the onset of the condition?

This method is known as a **Finite Impulse Response** model, in which you specify the length of the time window and how many time-points you want to estimate.

For this tutorial, we will be using the data from the `Pauli et al., 2016 paper <https://openneuro.org/datasets/ds000011/versions/00001>`__. The dataset analyzed in this paper can be found on OpenNeuro `here <https://openneuro.org/datasets/ds000011/versions/00001>`__.
