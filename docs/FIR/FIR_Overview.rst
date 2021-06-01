.. _FIR_Overview:

=============================
Finite Impulse Response (FIR)
=============================

----------------

Overview
********

If you have completed the tutorials on task-based fMRI studies, you are familiar with how **basis functions** are convolved with onset times to best fit the time-series. One drawback of this method is that a single best-fitting parameter is estimated for all of the instances of a particular condition; what if instead we wanted to examine the average time-course of the activity after the onset of the condition?

This method is known as a **Finite Impulse Response** model. You specify the length of the time window and how many time-points you want to estimate.
