.. _datalad_Tutorial:

=====================================
Datalad Tutorial and Pre-Registration
=====================================


.. note::

  This page is under construction. Please check back soon! (i.e., one week)

-----------

If you have ever downloaded a dataset from an open-access repository such as `OpenNeuro <https://openneuro.org/>`__, you have probably just clicked on the "Download" button next to whatever dataset you wanted to look at. This works in most cases, but it can be tedious to do for every single file, especially for dataset with dozens or hundreds of scans.

Another approach is to use Amazon Web Servces (aws) by using their `Command Line Interface <https://aws.amazon.com/cli/>`__; this requires downloading their "aws" command and then using it to download the entire dataset. After installing the command, you can use it to download an entire dataset in one go, which is more efficient than downloading each subject or file manually.

Yet another approach which combines the advantages of both of the above is called `datalad <https://handbook.datalad.org/en/latest/index.html>`__, a data management tool that allows you the flexibility to download specific files using the command line. Datalad is built on `git-annex <https://git-annex.branchable.com/>`__, a data synchronization tool that allows you to download the entire dataset structure without any of the actual data, and then decide which files to download individually, or whether to download all of the data. The following tutorial assumes that you are using a Macintosh operating system, although the same commands should work in Linux operating systems, or in Windows Powershell.

Installing datalad
******************

Instructions for how to install datalad can be found `here <https://handbook.datalad.org/en/latest/intro/installation.html>`__. Note that datalad requires Python 3, and that you also need to have Git and git-annex installed. The easiest way to install all of these packages and their dependencies is through `Homebrew <https://brew.sh/>`__, a package installation manager that we have used in other parts of this e-book. If you already have Git and git-annex, you can install datalad by typing:

::

  brew install datalad

That should be all you need to do. To check whether it has installed correctly, begin typing "datalad", and then press tab; If datalad is located in your path, it should autocomplete, and if you press enter, it should display the help file for the datalad command.

Note that you may encounter some path issues depending on how your Macintosh is set up. There are ways to debug this which can be found in the datalad installation link above; but for the rest of this tutorial, I will be using Neurodesk to show how to use datalad, which will get around many of the path and dependencies issues that can cause problems with some computers.
  

Demonstration with Neurodesk
****************************

To demonstrate how to use datalad, Neurodesk is a robust analysis platform which already comes pre-installed with the software package, as well as many other neuroimaging analysis packages as well. To download and install Neurodesk, follow the instructions `on their website <https://neurodesk.org/>`__, or review the :ref:`Neurodesk page <Neurodesk_Overview>` available in another part of this e-book. The advantages of Neurodesk, in addition to already having datalad and other essential software installed automatically for you, removes any of the issues with paths and dependencies that you may encounter with your customized personal computer setup. You can also easily move the results from the Neurodesk environment to your local machine through a portal directory called **neurodesktop-storage** that is created in your home directory when you install Neurodesk.

When you create a new Neurodesk instance, click on the computer screen icon in the lower left corner of the new window that appears. This will open a Terminal, in which you can type "datalad", which should be installed.

Once you find a suitable dataset on `OpenNeuro <https://openneuro.org>`__, click on the Download tab and see what the URL is for downloading the dataset using datalad. Then copy and paste that line of code into your Neurodesk terminal; for example, to download the Kelly et al. (2008) Flanker dataset, the code might be something like this:

::

   datalad install https://github.com/OpenNeuroDatasets/ds000102.git

Running this code in Neurodesk will download the dataset structure from OpenNeuro, but without downloading any of the actual data yet. If you navigate to the newly created ``ds000102`` directory and type ``ls``, you will see something like this:

.. figure:: datalad_OpenNeuroDataset.png

Note that you cannot view or manipulate any of the files using the usual commands, like fsleyes to view the images, or 3dcalc to take the average of two functional datasets.
