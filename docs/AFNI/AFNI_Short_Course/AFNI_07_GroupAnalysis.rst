.. _AFNI_07_GroupAnalysis:

=====================
AFNI Tutorial #7: Group Analysis
=====================

--------

Overview
***************

Our goal in analyzing this dataset is to generalize the results to the population that the sample was drawn from. In other words, if we see changes in brain activity in our sample, can we say that these changes would likely be seen in the population as well?

To test this, we will run a **group-level analysis** (also known as a **second-level analysis**). In AFNI, this means that we calculate the standard error and the mean for a contrast estimate, and then test whether the average estimate is statistically significant. We will be doing this group-level analysis in two ways: Using 3dttest, which uses only the contrast estimates in testing for statistical significance; and using 3dMEMA, which accounts for both the difference between the parameter estimates, and the variability of that contrast.


Using uber_ttest.py
*******************

Just as we used a :ref:`graphical user interface to preprocess the data <01_AFNI_Commands_uber_subject>`, we can also use a GUI to set up our group-level analyses. If you type the command ``uber_ttest.py`` from the command line and press return, you will see this:

.. figure:: 07_uber_ttest_GUI.png

The first field, "program", allows you to choose between ``3dttest`` and ``3dMEMA``. As noted above, 3dMEMA allows you to account for the variability of the estimate as well, in order to give more weight to those subjects who have lower variability in their estimates. For now, leave it as "3dttest".

In both the "script name" and "dset prefix" fields, enter ``Flanker_Inc-Con_ttest``. For the "mask dset", you can choose any one of the subject's ``mask_group+tlrc`` images located in their results directories, as they should be similar. If you want to be more rigorous, you can calculate an intersection of the masks using AFNI's ``3dmask_tool`` command. The command would look something like this:

::

  3dmask_tool -input <path/to/masks/> -prefix mask_intersection+tlrc -union
  
We will now select each of the subject statistical datasets, using wildcards. If you click on the button "get subj dsets" in the "datasets A" section, you will be prompted to select a representative stats dataset. Select any of the subjects' statistical datasets, and replace the last two numbers of the subject ID with two question marks (i.e., ``??``). Then click on "apply pattern". If all of the subjects were analyzed the same way and have the same directory structure, you should see 26 entries in the field below. Review them to make sure they are all there, and then click "OK".

.. figure:: 07_Select_Datasets.gif

At the bottom of the "datasets A" section, you will see a few additional fields. In "set name (group of class), write ``Inc-Con``, and in "data index/label" type ``7``.

Why 7? If you open a Terminal and navigate to any of the subjects' results directories (for example, sub-08), type

::

  3dinfo -verb stats.sub-08+tlrc
  
This will show all of the sub-briks in that dataset, along with their labels. In our current example, we are looking for the contrast estimates for ``incongruent-congruent``. The output of ``3dinfo`` shows that this is located in sub-brik 7. Any sub-brik that contains the label "Coef" means that it is a parameter (or contrast) estimate; the label "Tstat" indicates that it is a t-statistic. (Likewise, "Fstat" means that it is an F-statistic.)

.. figure:: 07_3dinfo_output.png

.. note::

  What do you think the number to the right mean? For example, -19.2 to 11.3878? Why are they different between the "Coef" and the "Tstat" sub-briks? Does this difference make sense?
  
  
When you have finished, your GUI should look like the one below:

.. figure:: 07_3dttest_setup.png

.. note::

  If you receive an error saying that one of the fields hasn't been filled in - but you can clearly see that it has been filled in - click on another field, and then try clicking the "Go" icon again.
  
  
Generating the Results
**********************

As with the uber_subject.py script, there are buttons at the top of the GUI for both generating the script and then running the script. First click on the icon that looks like a sheet of paper with lines on it, which will show you the command that has been generated. Review it to see how it has inserted all of your inputs into a command called 3dttest++, which will run the actual group-level analysis. Then click on the green "Go" icon to run the test. (It should take only a second.)

When it has finished, go back to your Terminal and type ``ls``. You will see a new directory called ``group_results``, and within that a folder called ``test.001.3dttest++``. Navigate into that folder, which contains the script that was used to generate the results ("Flanker_Inc-Con_ttest"), and another folder called ``test.results``, which contains the group-level output "Flanker_Inc-Con_ttest+tlrc". Load this in the afni viewer, and overlay it on top of the MNI152 template. Threshold the images to an uncorrected p-value of 0.001 (by right-clicking on the "p=" underneath the slider bar) and clusterize the data to only show clusters with an extent of 40 voxels or more; this will create an image like the one below. Does the location of the activation make sense, given the task and the paper this experiment was based on?

.. figure:: 07_GroupLevel_Results.gif

Using 3dMEMA
*************

Close the AFNI viewer, and then use the Terminal to navigate back to the directory containing your subjects. Go back to the uber_ttest.py GUI (or open a new one), and make the following changes:

1. Change the "program" from 3dttest++ to 3dMEMA.
2. Change the "script name" and "dset prefix" to ``Flanker_Inc-Con_MEMA``.
3. Click on "get subj dsets", and select a subject's statistical dataset that has the "REML" string (e.g., ``stats.sub-08_REML+tlrc``). Use the wildcards as above to select all of the subjects' REML datasets.
4. In the field "t-stat index/label (MEMA)", type ``8``. The sub-briks of the REML dataset, which should be in an order identical to the non-REML statistical dataset, indicate that sub-brik #8 is the t-statistic associated with the contrast estimate of "incongruent-congruent."

As before, click on the script generator icon, and then click on the green "Go" button. This model estimation will take longer, and you will see a progress report for each slice that has been analyzed; in total, it should take only a couple of minutes.

.. figure:: 07_3dMEMA_setup.png

When it has finished, you will see a new directory in the group_results folder called ``test.002.3dMEMA``, with a sub-directory called ``test.results``. Navigate to that folder, and overlay the results as before. Are the effects in the same location? Do these effects look stronger or weaker? Why?
