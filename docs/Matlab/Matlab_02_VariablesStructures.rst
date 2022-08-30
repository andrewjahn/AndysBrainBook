.. _Matlab_02_VariablesStructures:


============================================
Matlab Tutorial #2: Variables and Structures
============================================

.. note::
    Topics covered: Variables, structures, saving
    
    Commands used: save, clear, load


Overview of Variables and Structures
************************************

Once we create a variable or set of variables, we may want to store them for future use. For example, let's say that we have two variables, x and y, defined using the following code:

::

    x = 10
    y = 20
    
If we wanted to use these later, we can save them into a ``.mat`` file using the ``save`` command:

::

    save myVariables.mat x y
    
Note that the format of this command is ``save``, followed by a .mat file, which you can call anything you want. This is followed by a list of variables that you want to store in the .mat file. If you don't provide any variables after the .mat file, by default it will save all variables in the workspace into the .mat file.

If we want to clear the workspace of all of the currently defined variables, we can remove them using the ``clear`` command. You can remove a single variable, e.g.,

::

    clear x
    
Or you can clear all of the variables in the workspace by typing ``clear all``. Let's say that we have created the file ``myVariables.mat`` as above, and we have cleared all of the current variables in the workspace. We can recover them by using the ``load`` command:

::

    load myVariables.mat
    
Note that the variables of x and y are now present in your workspace, and you can use them in the command window as you like.

If you want to save variables into a file format that can be read as a text file, you can use the -ascii option. For example, let's say we had two columns of numbers, representing the onset of each trial (the first column), and the duration of the trial (the second column):

::

    Onsets = [0 2; 10 1; 16 2; 24 1]
    
These can be saved into a new text file:

::

    save('Onsets.txt', 'Onsets', '-ASCII')
    
We will return to this concept during the chapter on **scripting**.

As a last exercise, create another variable, z:

::

    z = 'fMRI'
    
This is a string, which we can save along with the x and y variables we created earlier:

::

    save myVars x y z

Instead of simply using the ``load`` command, we can assign the output from ``load`` into another variable, e.g.:

::

    Variables = load('myVars')
    
You will now see a new object we haven't seen before, called a **structure**:

::

    Variables = 

    struct with fields:

    x: 10
    y: 20
    z: 'fMRI'
    
 

Structures
**********

