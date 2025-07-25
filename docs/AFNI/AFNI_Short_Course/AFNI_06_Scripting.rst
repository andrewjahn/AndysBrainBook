.. _AFNI_06_Scripting:

===========================
AFNI Tutorial #6: Scripting
===========================

----------

Overview
********

After you've preprocessed and set up a model for a single run for a single subject, you will need to do the same steps for all of the runs for all of the subjects in your dataset. This may seem tedious but doable - we only have twenty-six subjects, and two runs per subject. You may think that it can be done over the course of a week or so; and you can always assign the task to a couple of Research Assistants.

This attitude is admirable, and if you take this approach you will be able to analyze all of the data eventually. But at some point you will run into two problems:

1. You will find that manually analyzing each run is not only tedious but prone to error, and the probability of making a mistake increases significantly as the number of runs to analyze also increases; and

2. For larger datasets - for example, eighty subjects with five runs each - this approach quickly becomes impractical.

An alternative is to **script** your analysis. Just as an actor has a script which tells him what to say and where to stand and where to move, so can you write a script that tells your computer how to analyze your datasets. This has the double benefit of automating your analyses and being able to analyze datasets of any size - the code for analyzing two subjects or two hundred is virtually identical.

First we will create a template that contains the code needed to analyze a single run, and then we will use a :ref:`for-loop  <Unix_05_ForLoops>` to automate the analysis for all of the runs. The idea is simple; and although the code can be difficult to understand at first, once you become more comfortable with it you will see how you can apply it to any dataset.

.. note::

  The following tutorial complements the Unix tutorial on :ref:`automating the analysis <Unix_09_AutomatingTheAnalysis>`. I recommend reading through that chapter if you need to review the Unix terms for scripting.

Creating the Template
*********************

When you analyzed the first run of ``sub-08``, you created a file called ``proc.sub_08``. This contained a list of AFNI commands, composed in a manner determined by the uber_subject.py GUI. Copy this file to the directory containing all of your subjects, and rename is to ``proc_Flanker.sh``. For example, if the ``proc.sub_08`` script was in the sub-08 directory structure generated by uber_subject.py, you could type the following from the directory containing all of your subjects:

::

  cp sub-08/subject_results/group.Flanker/subj.sub_08/proc.sub_08 proc_Flanker.sh
  
We will make the following changes to the script:

1. We will remove every reference to ``sub-08``, and turn those strings into a variable that is taken from an argument given to the script. For example, we will change the script so that if we execute it by typing ``tcsh proc_Flanker.sh sub-01``, it will replace the variable in our script with the string ``sub-01``, and analyze that subject's data.

2. We will replace the paths to be more generalizable.

To begin, open the proc_Flanker.sh file in a text editor such as TextWrangler. Scroll to lines 26-31, which contains the following code:

::

  # the user may specify a single subject to run with
  if ( $#argv > 0 ) then
      set subj = $argv[1]
  else
      set subj = sub_08
  endif
  
This is a :ref:`conditional statement <Unix_06_IfElse>` using tcsh syntax. The first few lines state that if the user provides an argument (i.e., an input), then set the variable "subj" to whatever the argument is (see the above text about making changes to the script, #1). If you look through the rest of the script, you will see numerous lines that contain the variable "$subj", which will be replaced by the argument. However, there are many instances - usually involving paths - that still have the string ``sub-08`` hard-coded into them. In order to make the script more flexibile and have it analyze the subject that we specify, we will need to replace these with the "$subj" variable. If you are using TextWrangler, click on "Search" from the menu at the top of the screen, and select "Find". In the "Find" field, type ``sub-08``, and in the "Replace" field, type ``${subj}``. 

Next, we will need to replace any absolute paths with a :ref:`relative path <Unix_04_ShellsVariables>`. As you can see in the script, there are several lines of code that contain paths starting with ``/Users/ajahn/Desktop/Flanker``. We will replace this with the $PWD variable, which is a shorthand for the path to the current working directory. This will ensure that the script will be adapted to the current computer's directory structure, and that no errors will be thrown due to the script being unable to locate where certain files are. From the TextWrangle Search and Replace screen, "Find" the string ``/Users/ajahn/Desktop/Flanker`` (or whatever the name of the path is which points to the directory containing your subjects), and "Replace" it with ``${PWD}``. Also replace on line 255 ``/Users/ajahn/aglobal`` (or whatever your username is) with ``~/abin``.

The template script with all of the edits can be found `here <https://github.com/andrewjahn/AFNI_Scripts/blob/master/proc_Flanker.sh>`__.

.. note::

  To speed up the analysis, I prefer to use the ``-mask`` option with the 3dDeconvolve command. For example, I would change line 299 of the script to: ``3dDeconvolve -input pb04.$subj.r*.scale+tlrc.HEAD  -mask mask_group+tlrc``.
  There are reasons against this, such as the fact that there may be systematic variations outside of the brain that you will miss by masking out the non-brain voxels. Nevertheless, using a mask speeds up the regression block considerably; and I would argue that if there are any "problem" voxels outside of the brain, they would be detected by inspecting the output of each of the preprocessing blocks.

Automating the Analysis
***********************

We will now use this updated preprocessing script in a for-loop to analyze all of the subjects in our dataset. Use this code:

::

  for i in `cat subjList.txt`; do
    tcsh proc_Flanker.sh $i;
    mv ${i}.results $i;
  done
  
This will run the script "proc_Flanker.sh" for each subject in the file "subjList.txt", using each consecutive line in the subjList.txt file as an argument each time the script runs.


This will run the preprocessing and regression for each subject, storing the output in a folder called ``<subjID>/<subjID>.results``, in which "subjID" stands for the subject name. Each analysis will take 5-10 minutes, depending on the speed of your computer.


Video
*****

For a video overview of scripting, click `here <https://www.youtube.com/watch?v=8M9repfRObc>`__.
