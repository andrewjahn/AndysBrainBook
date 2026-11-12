.. _datalad_Tutorial:

===================
Overview of DataLad
===================

.. note::

  The following tutorials are adapted from `The DataLad Handbook <https://handbook.datalad.org/en/latest/>`__, which covers all aspects of DataLad. That resource is written in an accessible, readable style that is an effective introduction for newcomers of any background. The chapters of this module have synthesized a few portions of that handbook to focus specifically on fMRI analysis. 

-----------

If you have ever downloaded a dataset from an open-access repository such as `OpenNeuro <https://openneuro.org/>`__, you have probably just clicked on the "Download" button next to whatever dataset you wanted to look at. This works in most cases, but it can be tedious to do for every single file, especially for datasets with dozens or hundreds of scans.

Another approach is to use Amazon Web Services (aws) by using their `Command Line Interface <https://aws.amazon.com/cli/>`__; this requires downloading their "aws" command and then using it to download the entire dataset. After installing the command, you can use it to download an entire dataset in one go, which is more efficient than downloading each subject or file manually.

Yet another approach which combines the advantages of both of the above is called `datalad <https://handbook.datalad.org/en/latest/index.html>`__, a data management tool that allows you the flexibility to download specific files using the command line. Datalad is built on `git-annex <https://git-annex.branchable.com/>`__, a data synchronization tool that allows you to download the entire dataset structure without any of the actual data, and then decide which files to download individually, or whether to download all of the data. The following tutorials assume that you are using a Macintosh operating system, although the same commands should work in Linux operating systems, or in Windows Powershell.

Installing datalad
******************

Instructions for how to install datalad can be found `here <https://handbook.datalad.org/en/latest/intro/installation.html>`__. Note that datalad requires Python 3, and that you also need to have Git and git-annex installed. The easiest way to install all of these packages and their dependencies is through `Homebrew <https://brew.sh/>`__, a package installation manager that we have used in other parts of this e-book. If you already have Git and git-annex, you can install datalad by typing:

::

  brew install datalad

That should be all you need to do. To check whether it has installed correctly, begin typing "datalad", and then press tab; If datalad is located in your path, it should autocomplete, and if you press enter, it should display the help file for the datalad command.

Note that you may encounter some path issues depending on how your Macintosh is set up. There are ways to debug this which can be found in the datalad installation link above; but for the rest of these tutorials, I will be using Neurodesk to show how to use datalad, which will get around many of the path and dependencies issues that can cause problems with some computers.

.. toctree::
   :maxdepth: 1
   :caption: Analyzing fMRI Data with DataLad

   DataLad_Short_Course/DataLad_01_Neurodesk
   DataLad_Short_Course/DataLad_02_PopulatingNewDataset
   DataLad_Short_Course/DataLad_03_RunAndRerun
   DataLad_Short_Course/DataLad_04_Collaboration
  
