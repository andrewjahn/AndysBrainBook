.. _DataLad_02_PopulatingNewDataset:

========================
Populating a New Dataset
========================

-----------
  
Version Control
***************

As mentioned earlier, one of DataLad's strengths is **version control**: The ability to document what changes happened at which time, and who made the changes - as well as a note explaining why the change was made. Access to all of the different versions also allows the user to revert to an earlier version if needed, either because the earlier version is determined to be superior to the most recent version, or to replicate the analysis that was done with that particular version. This is a common feature of many open science tools and websites, including GitHub and NeuroDesk. Having a digital trail of each edit means that any code and any experiment can be audited, by ourselves and by our colleagues. The field benefits from increase transparency.

Creating Datasets
*****************

What we did in :ref:`the previous tutorial <DataLad_01_Neurodesk>` was downloading and analyzing a publicly available dataset from `OpenNeuro <https://www.openneuro.org>`__. This was done using the **install** command, which can be used to download files from any website. In this tutorial, we will learn how to create and populate a datset from scratch, as well as how to document any changes, also known as **commits**.

The basic format of creating a new folder is:

::

  datalad create [--description "..."] [-c <config options] PATH

For example, we can create a new directory by typing:

::

  datalad create --description "This is the first directory I have created using datalad" -c text2git myDataDirectory

We will create a new directory, ``myDataDirectory``. Apparently there is nothing inside the directory, as we can see if we type ``ls myDataDirectory``. However, we can use the ``-a`` option with ``ls`` to show hidden files with a ``.`` prefix:

::

  cd myDataDirectory
  ls -a
  .  ..  .datalad  .git  .gitattributes


The ``text2git`` option stores all of the text files in this directory in Git, which allows them to be edited directly; otherwise, the files would have to be unlocked with ``datalad unlock`` or ``datalad run``, which will be discussed in the next tutorial. The other hidden files are the directories ``.datalad`` and ``.git``, which contain configuration files that allow the directory to be manipulated with ``git`` commands, as well as a unique identifier called a **shasum**, a forty-digit hexadecimal code. For now, all you need to know about this hidden files is that they should stay hidden - do not edit, tamper with, or delete them, or else you will not be able to use Git with the directory.

We can see other commits, or edits, by typing ``git log``. The output looks something like this:

::

  jovyan@neurodesktop-2026-10-07:~/Desktop/myDataDirectory$ git log
  commit c7527701364061feaa3a3d91c627b7ee7bfeda23 (HEAD -> master)
  Author: Neurodesk User <user@neurodesk.org>
  Date:   Fri Nov 13 14:00:16 2026 +0000

    Instruct annex to add text files to Git

  commit af3d759562e3dca7f7d338ed03ab645ff1f070d3
  Author: Neurodesk User <user@neurodesk.org>
  Date:   Fri Nov 13 14:00:15 2026 +0000

    [DATALAD] new dataset

Notice that since this directory was created in NeuroDesk, the **Author** is specified as "Neurodesk User". There are a couple of commits, identified by their shasums, along with a message saying "Instruct annex to add text files to Git". This last part refers to the ``text2git`` command we used earlier with datalad.


Populating the Dataset
**********************

After creating the directory, we can populate it with subdirectories and files. For example, if we go to OpenNeuro and right click on the download button next to a file, we can paste that link into a ``wget`` command, which downloads files from a URL:

::

  mkdir myData
  cd myData
  mkdir myAnat
  cd myAnat
  wget -q https://s3.amazonaws.com/openneuro.org/ds002422/sub-03/anat/sub-03_T1w.nii?versionId=CyedfK.R1wleGw1ghETHb_mQSnrvPFec -O myAnat.nii.gz

This will create a directory, ``myData``, and a subdirectory, ``myAnat``. The ``wget`` command is then used with the pasted URL link to download the anatomical dataset, and the ``-O`` option saves it as a more readable filename; in this case, ``myAnat.nii.gz``.
