.. _Matlab_01_Navigation::

=========================================
Matlab Tutorial #1: Navigation & Matrices
=========================================

.. note::
    Topics covered: Directories, navigation, matrices, strings, integers
    
    Commands used: pwd, cd, ls


Overview of the Matlab Interface
********************************

When you first open Matlab, you will notice that it has four distinct sections: Four windows, and a ribbon of buttons at the top, similar to the layout of Microsoft Word. The window on the left is the Navigation window, the central window is the Command Window, and the upper-right window is the Workspace. The Navigation window contains a list of all the folders located within the folder you are currently in; for example, if I am in my home directory, you may see sub-folders such as Documents, Desktop, Music, and so on.

The Command Window is where you can directly type commands and see the output of those commands. As you will see below, these can be commands for navigating to different directories, listing the contents of the current directory, or creating variables that can be used in scripts. Most of what we will be doing in this tutorial requires you to use the Command Window. The Workspace window contains **variables** that you have created in your current session. The concept of variables will be discussed in a later chapter.

.. figure:: 01_Matlab_Layout.png

Navigating with Matlab
**********************

Like other operating systems, Matlab organizes folders and files using a directory tree - also known as a directory hierarchy, or directory structure. At the top of the hierarchy is a folder called ``root``, written as a forward slash (``/``). All other folders (also known as directories) are contained within the ``root`` folder, and those folders in turn can contain other folders.

Think of the directory hierarchy as an upside-down tree: ``root`` is the base of the tree, and all of the other folders extend from it, just as branches extend from the trunk.

.. figure:: UnixTree.png

    Root, symbolized by a forward slash (``/``), is the highest level of the directory tree; it contains folders such as ``bin`` (which contains binaries, or Matlab commands such as pwd, cd, ls, and so on), ``mnt`` (which shows any currently mounted drives, such as external hard drives), and ``Users``. These directories in turn contain other directories - for example, ``Users`` contains the folder ``andrew``, which in turn contains the ``Desktop``, ``Applications``, and ``Downloads`` directories. This is how folders and files are organized within a directory tree.
    

To navigate around your computer, you will need to know the commands ``pwd``, ``cd``, and ``ls``. ``pwd`` stands for “print working directory”; ``cd`` stands for “change directory”; and ``ls`` stands for “list”, as in “list the contents of the current directory.” This is analogous to pointing and clicking on a folder on your Desktop, and then seeing what’s inside. Note that in these tutorials, the words “folder” and “directory” are used interchangeably.

.. figure:: Desktop_Folder.png

    Navigation in Matlab is the same thing as pointing and clicking in a typical graphical user interface. For example, if you have the folder "ExperimentFolder" on my Desktop, you can point and double-click to open it. You can do the same thing by typing ``cd ~/Desktop/ExperimentFolder`` in the Terminal and then typing ``ls`` to see what's in the directory.

Introduction to Matrices
************************

In addition to being able to navigate from the command line and the ability to host software programs such as SPM12 and the CONN toolbox, Matlab allows the user to create and manipulate **matrices**. For example, we can create a matrix simply by typing:

::

    x = 1
    
Once you press enter, this will automatically print output to the terminal that says "x = 1". Note that "x" is now stored as a **variable** in the Workspace window; you can see both the name of the variable, and its value. The reason we call these objects "variables" is that they can contain any value, and can be updated in a command structure such as a **for-loop**, which will be discussed in a later chapter.

If you ever want to find out the value contained within the variable, you can simply type ``x`` at the command line and press enter, or you can look in the Workspace. For even more detailed information about the variable, you can type:

::

    whos x
    
This will display the following:

::

  Name      Size            Bytes  Class     Attributes
  x         1x1                 8  double              

For now we will ignore the last three fields, and focus on the first two. The first column, "Name", is simply the name of the variable; but notice that the second column, "Size", contains the value "1x1". Even a single value stored in a variable - in this case, the value "1" contained in the variable "x" - is labeled by Matlab as a 1x1 matrix. If we wanted to create a variable that spanned multiple dimensions, such as a 4x3 matrix, we could type something like the following:

::

    a = [1 2 3; 4 5 6; 7 8 9; 10 11 12];

.. note::

    In this line of code, we ended it with a semicolon (``;``). This suppresses the output from being automatically printed to the terminal, while still storing the value in the variable.
    
Notice that we now have a new variable in our Workspace window called "a", which is a 4x3 matrix. If you type 

Video
-----

Click `here <https://www.youtube.com/watch?v=TQqJD-v6glE&list=PLIQIswOrUH69xOiblvvEz5KBwWaNRMEUp&index=2>`__ to see a video overview of the commands cd, ls, and pwd - the basic commands you will need to navigate around your directory tree.


-------------

Exercises
---------

When you're done watching the video, try the following exercises:

1.  Type ``ls ~`` and note what it returns; they type ``ls ~/Desktop``. How are the outputs different? Why?

2.  Navigate to the Desktop by typing ``cd ~/Desktop``. Type ``pwd`` and note what the path is. Then create a new directory using the ``mkdir`` command, choosing a name for the directory on your own. Navigate into that new directory and think about how your current path has been updated. Does that match what you see from typing ``pwd`` from your new directory?

3.  Define the terms ``cd``, ``ls``, and ``pwd`` in your own words. 
