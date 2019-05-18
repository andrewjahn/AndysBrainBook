.. _fMRI_06_Scripting.rst

fMRI Tutorial #6: Scripting
===============


.. note::

  This section is under construction as of 05/14/2019. Check back soon!
  
-----------

Overview
********

After you've preprocessed and set up a model for a single run for a single subject, you will need to do the same steps for all of the runs for all of the subjects in your dataset. This may seem tedious but doable - we only have twenty-six subjects, and two runs per subject. You may think that it can be done over the course of a week or so; and you can always assign the task to a couple of Research Assistants.

This attitude is admirable, and if you take this approach you will be able to analyze all of the data eventually. But at some point you will run into two problems:

1. You will find that manually analyzing each run is not only tedious but prone to error, and the probability of making a mistake increases significantly as the number of runs to analyze also increases; and

2. For larger datasets - for example, eighty subjects with five runs each - this approach quickly becomes impractical.

An alternative is to **script** your analysis. Just as an actor has a script which tells him what to say and where to stand and where to move, so can you write a script that tells your computer how to analyze your datasets. This has the double benefit of automating your analyses and being able to analyze datasets of any size - the code for analyzing two subjects or two hundred is virtually identical.

First we will create a template that contains the code needed to analyze a single run, and then we will use a for-loop to automate the analysis for all of the runs. The idea is simple; and although the code can be difficult to grasp at first, once you become more comfortable with it you will see how you can apply it to any dataset.

Creating the Template
*******

When you analyzed the first run of ``sub-08``, a directory called ``run1.feat`` was created. Within that directory, there are several files and sub-directories, a few of which we covered in the tutorial on preprocessing. You will see a file called ``design.fsf`` which contains all of the code transcribed from the FEAT GUI into a text file. This is the code that FSL runs to do each preprocessing and modeling step. If you open up the design.fsf file in a text editor and the FEAT GUI that you used to create the design.fsf file, and you compare them side-by-side, you can see where the data entered into the FEAT GUI is written into the design.fsf file.


.. figure:: FEAT_Design_File.png


If you open up the FEAT GUI, click on the ``Load`` button at the bottom of the screen, and select the design.fsf file in the run1.feat directory, it will change all of the settings to what you had used to run FEAT.

In the previous tutorials we ran FEAT separately for preprocessing and for model fitting. We will now create a template that combines both of these steps by selecting "Full Analysis" from the dropdown menu of the FEAT GUI.

First, remove the current run1.feat directory by typing ``rm -r run1.feat``. Then open up the FEAT GUI by typing ``Feat_gui`` from the command line. Using the previous tutorials as a guide, fill in all of the required fields for both preprocessing and model fitting.

Once you have filled in all of the fields, instead of clicking the ``Go`` button, click ``Save`` and label the file ``design_run1``. This will save out several files with extensions such as "con", "mat", and "png", but it is the file design_run1.fsf that we will be using for our script.

.. note::

  The rest of the Scripting process assumes that you are already familiar with :ref:`for-loops <Unix_05_ForLoops>`, :ref:`conditional statements <Unix_06_IfElse>`, and the basics of :ref:`scripting <Unix_07_Scripting>` as well as sed (FILL IN WHEN SED TUTORIAL IS FINISHED). If not, review those tutorials before going on.
  
  

Open design_run1.fsf in nano and take a look at all of the options that have been filled in. Our goal is to make this a template that can be run for any subject, with slight alterations that will be changed in a for-loop. In this case, the only thing we need to change is the subject name - the rest of the options will be the same for each subject.

Running the Script
**********

Copy this design_run1.fsf file to the directory containing your subjects.
