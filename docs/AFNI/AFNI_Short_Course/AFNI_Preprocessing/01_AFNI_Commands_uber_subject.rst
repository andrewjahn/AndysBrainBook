.. _01_AFNI_Commands_uber_subject:

=============
Chapter 1: AFNI Commands and uber_subject.py
=============

---------------

Overview
********

Among all of the fMRI analysis packages, AFNI has the reputation of being the most difficult to learn. Although this may have been true in the past, the AFNI developers have worked hard over the last few years to make their software easier to learn and easier to use: in addition to the viewer, recent versions of AFNI contain other graphical user interfaces that can be accessed through the commands ``uber_subject.py`` and ``uber_ttest.py``. These GUIs are used to create scripts which automate both the preprocessing and model setup for each subject.

Before we discuss those commands, however, we will review the basics of a typical AFNI command. The "uber" scripts, after all, simply compile large numbers of commands together in an order that processes the data. You will also be using individual AFNI commands to perform more advanced analyses, such as region of interest analysis.


AFNI Commands
*************

AFNI commands are similar to :ref:`Unix commands <Unix_Intro>`: They typically require at least one **argument**, or input, and they also usually require you to specify what to call the output of the command.

Let's take skull-stripping, for example - a common preprocessing step that removes the skull from the brain. The AFNI command to do this step is called ``3dSkullStrip``. Navigate to the directory ``sub-08/anat``, and then type "3dSkullStrip" and press enter. You will see the following message:

.. figure:: 01_SkullStripErrorMessage.png

Usually, just typing the command without any arguments would print the help file by default. We are required here to specify an additional flag, ``-h``, to print the help; type ``3dSkullStrip -h`` and press return. You will notice that a lot of text is printed to the screen - more than can be displayed in the Terminal all at once. You will need to scroll up to see the entire help file, which you might be fine with. 

If, on the other hand, you would like to see the help file in a format that is easier to read, type ``3dSkullStrip -h | less``. The vertical bar indicates that the output from the command to the left of the bar - that is, "3dSkullStrip -h" - should be piped into the ``less`` command, which allows you to page up and down through the help file. When you are in this "paging window", type "d" to go down one page; "u" to go up one page; and the up and down arrows to go up or down by one line. To search the help file, type a forward slash (``/``) followed by the text you want to find, and press enter. To exit the paging window, press "q".

[Insert gif showing this]

The documentation and help files are some of AFNI's greatest strengths. The usage of each command is clearly outlined, and the reasons for using different options are explained in detail. Sample commands are given to cover different scenarios - for example, if the skull-strip leaves too much skull in the output image, you are encouraged to use an option such as "-push_to_edge".

The most basic usage of 3dSkullStrip is to use an "-input" flag to specify the anatomical dataset that will be stripped. For example,

::

  3dSkullStrip -input sub-08_T1w.nii.gz
  


