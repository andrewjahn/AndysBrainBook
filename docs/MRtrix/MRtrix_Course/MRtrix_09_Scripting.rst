.. _MRtrix_09_Scripting:

=============================
MRtrix Tutorial #9: Scripting
=============================

--------------

Overview
********

After you've preprocessed and set up a model for a single run for a single subject, you will need to do the same steps for all of the runs for all of the subjects in your dataset. This may seem tedious but doable - we only have forty-two subjects, and one run of diffusion data per subject. You may think that it can be done over the course of a week or so; and you can always assign the task to a couple of Research Assistants.

This attitude is admirable, and if you take this approach you will be able to analyze all of the data eventually. But at some point you will run into two problems:

1. You will find that manually analyzing each run is not only tedious but prone to error, and the probability of making a mistake increases significantly as the number of runs to analyze also increases; and

2. For larger datasets - for example, eighty subjects with five runs each - this approach quickly becomes impractical.

An alternative is to **script** your analysis. Just as an actor has a script which tells him what to say and where to stand and where to move, so can you write a script that tells your computer how to analyze your datasets. This has the double benefit of automating your analyses and being able to analyze datasets of any size - the code for analyzing two subjects or two hundred is virtually identical.

First we will create a template that contains the code needed to analyze a single run, and then we will use a :ref:`for-loop  <Unix_05_ForLoops>` to automate the analysis for all of the runs. The idea is simple; and although the code can be difficult to understand at first, once you become more comfortable with it you will see how you can apply it to any dataset.

.. note::

  The following tutorial complements the Unix tutorial on :ref:`automating the analysis <Unix_09_AutomatingTheAnalysis>`. I recommend reading through that chaper if you need to review the Unix terms for scripting.

Creating the Template
*********************

