.. _Appendix_F_ParametricModulation:

=================================
Appendix F: Parametric Modulation
=================================

------------

Overview
********

This appendix demonstrates how to set up a **parametric modulation** analysis in FSL. An overview of what parametric modulation is can be found :ref:`here <PM_Overview>`, along with code for downloading the data. If you already have `Amazon Web Services <https://aws.amazon.com/cli/>`__ installed, then you can open a Terminal and type the following:

::

  aws s3 sync --no-sign-request s3://openneuro.org/ds000005 ds000005-download/
  mv ~/Downloads/ds000005-download ~/Desktop/Gambles
  
This will create a new folder on your Desktop called ``Gambles``, which we will use to store the data and the output from our analyses.

Preprocessing the Data
**********************

To prepare the data for a parametric modulation analysis, we will create a preprocessing pipeline similar to the one we used for the example dataset :ref:`here <fMRI_Intro>`. We will first create timing files that contain onsets for the Gamble, the parametric value for the potential Gain, and the parametric value for the potential Loss, for each run; in total, we will create nine regressors.

Download 
