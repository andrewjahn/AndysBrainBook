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

Similar to the integers and strings we have been using so far, a **structure** is another data type. In this case, however, it stores other data types as well, and organizes them into containers called **fields**. The structure we just created, ``Variables``, has three fields. These fields, in turn, can contain sub-fields, and those sub-fields can contain sub-fields, and so on, depending on your needs.

For example, let's say that we have a subject, and we have that person's name, profession, hobby, and heartrate. We could create his structure using this code:

::

    subject.name = 'Andy';
    subject.profession = 'Teacher';
    subject.hobby = 'Running';
    subject.Heart_rate = '60';
    
Similarly, we could type this all on one line to achieve the same end:

::

    subject = struct('name', 'Andy', 'profession', 'Teacher', 'hobby', 'Running', 'Heart_rate', 60)
    
Let's say that we wanted to change the ``name`` field so that it now contains two components, a ``First`` and ``Last`` name. If you try typing ``subject.name.first='Andy'``, it will throw an error. We could remove the ``subject`` structure completely by typing ``clear subject`` and starting over, or we could remove that specific field by using the ``rmfield`` command:

::

    subject = rmfield(subject, 'name')
    
Note that this command takes two arguments, the name of the structure (i.e., ``subject``), and the field we want to remove (i.e., ``name``). The first part of this command, ``subject = ``, will overwrite the structure, minus the field you just removed. We can then create a new structure within the structure, a field called ``name`` with two sub-fields, ``First`` and ``Last``:

::

    subject.name.First = 'Andy';
    subject.name.Last = 'Last';
    
Now when you type the variable name ``subject``, you will see the structure ``name`` located within the current structure:

::

    subject = 

  struct with fields:

    profession: 'Teacher'
         hobby: 'Running'
    Heart_rate: 60
          name: [1×1 struct]
          
You can add as many fields as you want; there are no limits to how many fields the structure can contain.

The SPM.mat File
&&&&&&&&&&&&&&&&

When you begin analyzing fMRI data with SPM12, at a certain point you will generate a file called ``SPM.mat``. These files will be generated both at the first-level per subject, or at the group-level. Let's begin with an SPM.mat file generated at the first-level. Click `here <https://github.com/andrewjahn/PSYCH808>`__ to access the Psych808 repository, and then click on the file ``SPM_SingleSubj.mat``. Then, click on the ``Download`` button to download it to your computer. Move the file to your Desktop directory by typing the following in the Matlab terminal:

::

    movefile('~/Downloads/SPM_SingleSubj.mat', '~/Desktop')
    cd ~/Desktop/
    
Then, load the .mat file by typing ``load SPM_SingleSubj.mat``. There will be a new variable in the workspace called ``SPM``, which is a 1x1 structure. From the command line, type ``SPM``, and see the following output:

::

    SPM = 

    struct with fields:

        xY: [1×1 struct]
       xBF: [1×1 struct]
     nscan: [146 146]
      Sess: [1×2 struct]
    factor: []
       xGX: [1×1 struct]
        xM: [1×1 struct]
        xX: [1×1 struct]
       xVi: [1×1 struct]
     SPMid: 'SPM12: spm_spm (v7738)'
     xsDes: [1×1 struct]
       swd: '/Users/ajahn/Desktop/Flanker/sub-01/1stLevel'
      xVol: [1×1 struct]
     Vbeta: [1×6 struct]
    VResMS: [1×1 struct]
        VM: [1×1 struct]
      xCon: [1×4 struct]
      
Many of these fields have unintelligible abbreviations, but over time you will become more familiar with them.
