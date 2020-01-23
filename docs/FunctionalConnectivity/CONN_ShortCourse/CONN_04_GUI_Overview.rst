.. _CONN_04_GUI_Overview:

========================
Chapter #4: The CONN GUI
========================

------------------

.. The following assume that a directory called CONN_Demo has already been created and is placed on the Desktop.

Overview
********

One of CONN's major advantages is its graphical user interface (GUI). Virtually everything that you will need to do can be done from the GUI, and CONN's layout is straightforward and easy to use.

As with an fMRI package like SPM, though, the reliance on the GUI comes at the expense of some flexibility. Later on during the module on scripting we will see how to write Matlab code that allows you to batch certain analyses and access data that isn't available from the interface. For most purposes, however, the CONN GUI works very well, and is particularly accessible for newcomers.

The first step of any CONN analysis is to create a new **project**. This will generate a Matlab file that contains fields reflecting all aspects of the experiment that have been changed from the GUI, such as options, names of files, and what data has been loaded. Let's say, for example, that we click on the ``New`` button and name our project ``conn_Arithmetic_Project``. Save it into the ``CONN_Demo`` folder, and then go back to the Matlab terminal. Navigate to that folder, and notice that it now contains a .mat file called ``conn_Arithmetic_Project.mat``. You can load this file by typing:

::

  load conn_Arithmetic_Project
  
There is now a new variable in your workspace called ``CONN_x``. This is a **structure file** that contains all of the fields of your experiment, similar to how a job file in SPM contains information on all of the changes you made in the GUI. In the figure below, I have already filled in a few of the fields in the GUI, such as the RT, number of subjects, and FWHM:

.. figure::

  04_CONN_MatFile.png

.. note::

  Those with experience using SPM may find it useful to review the chapter on :ref:`scripting in SPM <SPM_06_Scripting>`. The concepts are similar, and the newcomer to CONN may find it easier to understand what the .mat file does once he reviews how the same idea is carried out in SPM.
  
As you make changes to the project during preprocessing and certain analyses, you can overwrite the project file at any time by hovering your cursor over the "Project" menu and selecting "Save As". You can then close the CONN GUI if you need to, re-open it at a later time, and load your project by clicking the "Open" button and selecting the project file that you created.


Other Options
^^^^^^^^^^^^^

In order to make the CONN GUI easier to read, some users may want to change its appearance. Hover your mouse over the "Tools" menu, and select "GUI options." This will open a window that allows you to do things like increase the font size or change the color scheme. "Enable help tips" allows to you choose whether to have CONN automatically display help text when you hover your mouse over certain options. The "Background anatomical image" and "Background reference atlas" options allow you to specify the template that is used to project your results onto; you will see how the results are displayed once we cover 1st-level analysis, and if you don't like the default reference volume, you can choose a different one.

.. figure::
  
  04_GUI_Options.png

The Help Menu
^^^^^^^^^^^^^

As you use the CONN toolbox more, you will encounter errors that are specific to your analysis. The "Help" menu contains links to the CONN Manual (which can also be found `here <https://web.conn-toolbox.org/resources/documentation>`__) and other web resources. The "Search help" option opens a search menu to filter forum posts by topic. For example, if you want to see every post that mentioned GSR (Global Signal Regression), you just enter it in the filter field:

.. figure::

  04_Forum_Search.png
  
