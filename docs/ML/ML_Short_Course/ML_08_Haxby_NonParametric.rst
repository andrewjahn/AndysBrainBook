.. _ML_08_Haxby_NonParametric:

=====================================================
Machine Learning Tutorial #8: Non-Parametric Analysis
=====================================================

---------------

Overview
********

In the previous chapter, we used a t-test to determine whether there were significant differences in classification accuracies compared to chance. While this approach has been used by several researchers in the past (e.g., Li et al., 2009), the nature of the classification accuracies violates some of the assumptions of the t-test, which is a parametric test.

For example, this test assumes that the values are normally distributed around zero; in the case of classification accuracies, however, all of the numbers are constrained to be positive.

To remedy this, we can use other analyses. Here, we will review how to do non-parametric analyses with a toolbox called Statistical non-Parametric Mapping (SnPM).

Downloading SnPM
****************

SnPM can be downloaded by clicking on `this link <http://www.nisox.org/Software/SnPM13/>`__ and clicking on ``Registration``. You will be required to fill out a form with your email address. When you have finished filling out the form, you will be able to download the package. Unzip it, open a Terminal, navigate to the folder ``~/spm12/toolbox/``, and type:

::

  mv ~/Downloads/SnPM-devel-SnPM13.1.08 SnPM13
  
Then open a Matlab terminal, click on the ``Set Path`` button, and click ``Add Folder``. Select the folder ``SnPM13`` in the toolbox directory, click ``Save``, and then close the window.

From the Matlab terminal, open SPM by typing ``spm fmri``. Click on ``Batch``, then select ``SPM -> Tools -> SnPM -> Specify -> Multisub: One-sample T-test on diffs/contrasts``. This will open a new editor window.

From the Matlab terminal, navigate to the Haxby_Data directory, and create a new directory for our non-parametric results by typing ``mkdir 2ndLevel_GroupResults_SnPM``. Select this as the Analysis Directory in the Batch Editor window, and for Images to Analyze, select the smoothed and warped res_minus_chance images. Then click the green "Go" button. This will specify the model.

Then go back to the Batch Editor window, click on ``SPM -> Tools -> SnPM -> Compute``, and select the SnPMcfg.mat file. Click the green "Go" button.
