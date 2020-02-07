.. _CONN_02_DataDownload:

================================
Chapter #2: Downloading the Data
================================

------------------

Overview
********

The data we will use for this tutorial comes from the open-access repository `Openneuro.org <https://openneuro.org/>`__. From the Openneuro homepage, click on ``Dashboard``, which will load all of the available datasets. Select the dataset "fMRI: resting state and arithmetic task", which should be listed in the first page that comes up. If you are unable to find it, use the search bar to look for the string "arithmetic"; it should be the first result that is returned.

Click on the ``sub-01`` folder, and then click on ``anat``. Click the ``Download`` button next to the file "sub-01_T1w.nii", which will begin downloading the file. Next, click on the ``func`` folder to look at its contents, and download the file "sub-01_task-rest_bold.nii.gz".

If these files have been downloaded to your ``Downloads`` folder, open a Matlab terminal and type the following:

::

  cd ~/Desktop
  mkdir CONN_Demo
  
This navigates to your Desktop and creates a new folder called ``CONN_Demo``. This is where we will store our data and do our analyses. To organize the data into two folders within the CONN_Demo directory, type the following from your Matlab terminal:

::

  cd CONN_Demo
  mkdir func
  mkdir anat
  
Which will create the folders ``func`` and ``anat`` within the CONN_Demo directory. To move your downloaded files into their corresponding directories, type the following:

::

  movefile('~/Downloads/sub-01_task-rest_bold.nii.gz', 'func')
  movefile('~/Downloads/sub-01_T1w.nii', 'anat')
  
Type ``ls anat`` and ``ls func`` to make sure that the functional data is now located in the func directory, and that the anatomical data is located in the anat directory. If so, you are now ready to begin using the CONN toolbox, which we now turn to.
