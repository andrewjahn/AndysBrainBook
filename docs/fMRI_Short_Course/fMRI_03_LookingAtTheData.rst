.. _fMRI_03_LookingAtTheData:

================
fMRI Tutorial #3: Looking at the Data
================

Now that you've downloaded the dataset, let's see what it looks like. If the dataset has been downloaded to your Download directory, navigate to the Desktop and type the following:

::

    mv ~/Downloads/ds000102_0001/ Flanker
    
Which will rename the folder to Flanker and put it on your Desktop.


.. figure:: Move_Flanker_Folder.png

    After downloading the Flanker dataset, type the command above to move it to your Desktop.
    
    
As you saw in the previous ref:`Data Download page <fMRI_01_DataDownload>`, the dataset has a standardized structure: Each subject folder contains an anatomical directory and a functional directory labeled ``anat`` and ``func``, and these in turn contain the anatomical and functional images, respectively. (The ``func`` directory also contains **onset times**, or timestamps for when the subject did either a Congruent or Incongruent task.) This format is known as `BIDS <http://bids.neuroimaging.io/>`__, or Brain Imaging Data Structure, which makes it easy to organize and find your data.

.. figure:: Flanker_DataStructure.png

    
