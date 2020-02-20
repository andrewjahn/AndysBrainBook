.. _CONN_12_Scripting:

======================
Chapter #12: Scripting
======================

------------------

.. note::

  A template script for this tutorial was provided by the `Chronic Pain & Fatigue Research Center <https://medicine.umich.edu/dept/chronic-pain-fatigue-research-center>`__ of the University of Michigan. In particular, thanks to Chelsea Kaplan and Tony Larkin.

Overview
********

After you’ve preprocessed and set up a model for a single run for a single subject, you will need to do the same steps for all of the runs for all of the subjects in your dataset. This may seem tedious but doable - we only have twenty-six subjects, and two runs per subject. You may think that it can be done over the course of a week or so; and you can always assign the task to a couple of Research Assistants.

This attitude is admirable, and if you take this approach you will be able to analyze all of the data eventually. But at some point you will run into two problems:

1.You will find that manually analyzing each run is not only tedious but prone to error, and the probability of making a mistake increases significantly as the number of runs to analyze also increases; and

2. For larger datasets - for example, eighty subjects with five runs each - this approach quickly becomes impractical.

An alternative is to **script** your analysis. Just as an actor has a script which tells him what to say and where to stand and where to move, so can you write a script that tells your computer how to analyze your datasets. This has the double benefit of automating your analyses and being able to analyze datasets of any size - the code for analyzing two subjects or two hundred is virtually identical.


Scripting in the CONN Toolbox
*****************************

Structures, Fields, and Variables
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The CONN Toolbox comes with a command called ``conn_batch``, which takes a **structure** as an argument. A structure is a Matlab data type that organizes several variables into containers called **fields**. The structure fields that are expected by the ``conn_batch`` command are the following (taken from the conn_batch help, which can also be found on the `CONN scripting page <https://sites.google.com/view/conn/resources/conn_batch?authuser=0>`__):

::

   filename          : conn_*.mat project file (defaults to currently open project)
   subjects          : Subset of subjects to run processing steps or define parameters for (defaults to all subjects)
   parallel          : Parallelization options (defaults to local procesing / no parallelization)
   Setup             : Information/processes regarding experiment Setup and Preprocessing
   Denoising         : Information/processes regarding Denoising step
   Analysis          : Information/processes regarding first-level analyses
   Results           : Information/processes regarding second-level analyses/results
   QA                : Information/processes regarding Quality Assurance plots
   
In other words, the variable that we will pass as an argument to the ``conn_batch`` command - a variable that we will call **batch** from now on - requires the fields listed above. If you recall from the earlier chapters, there were tabs in the CONN GUI that were labeled Setup, Denoising, Analysis, and Results; these fields will also contain sub-fields that specify the values that we entered into the GUI. For example, the field:

::

  batch.Setup.RT=3.56
  
specifies that the RT for this experiment is 3.56 seconds.

The CONN_x Structure
^^^^^^^^^^^^^^^^^^^^

The other fields and sub-fields can be found on the CONN scripting page, along with explanations about what each value and option means. But the easiest way to create and modify a structure is by generating one through the GUI.

In fact, you already have created such a structure, even if you weren't aware of it. Each time you save your project, a .mat file is created. This .mat file can be **loaded** into the Matlab terminal by using the ``load`` command. For example, if you have been following along with the tutorials up to this point, you have created a .mat file called ``conn_Arithmetic_Project.mat``. You can load it by typing:

::

  load conn_Arithmetic_Project.mat
  
This returns a structure in your workspace called ``CONN_x``. When you type ``CONN_x``, you will see the following text returned:

.. figure:: 11_CONN_x.png

Each of these fields contain the values that you entered into each tab. For example, we can display the values that were entered in the ``Setup`` tab by typing ``CONN_x.Setup``, which returns several fields (only the first few are displayed in the figure below):

.. figure:: 11_CONN_x_Setup.png

Note the correspondence between the values entered in these fields, and the values that are entered when you load the ``conn_Arithmetic_Project.mat`` file. If we want, we can edit the values within the CONN_x structure from the Matlab terminal, save it as a .mat file, and then load it into the CONN toolbox. The edits we make in the Matlab terminal should then be reflected in the GUI.

Try this by typing the following line of code in the Matlab terminal:

::

  CONN_x.Setup.RT = 1
  
This replaces the original value of 3.56 with a value of 1. We can then save the updated CONN_x structure into our .mat file by typing:

::

  save('conn_Arithmetic_Project.mat', 'CONN_x')
  
This uses the Matlab ``save`` command, which requires two arguments: The .mat file to be written, and any values or structures to be saved into it. In this case, we save the CONN_x structure into the .mat file. If you now load it from the CONN GUI (by clicking on ``Project -> Open`` and selecting the file), the Basic Information tab should be updated with the new Repetition Time that you specified in the Matlab terminal.

The CONN_x structure also specifies menu options. For example, the Batch.Setup section of the CONN help page says about the field ``acquisitiontype``:

::

  acquisitiontype : 1/0: Continuous acquisition of functional volumes [1] 
  
This is a binary variable, with 1 signalizing a continuous acquisition of functional volumes (which is the default, as indicated by the brackets). If we set this to 0, then it will be the other option that is available, which is spare sampling. As an exercise, set the field ``CONN_x.Setup.acquisitiontype`` to 0, save the CONN_x structure into the .mat file, and reload the .mat file. What do you observe has changed?
