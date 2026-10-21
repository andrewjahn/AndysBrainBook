.. _datalad_Tutorial:

=====================================
Datalad Tutorial and Pre-Registration
=====================================


.. note::

  This page is under construction. Please check back soon! (i.e., one week)

-----------

If you have ever downloaded a dataset from an open-access repository such as `OpenNeuro <https://openneuro.org/>`__, you have probably just clicked on the "Download" button next to whatever dataset you wanted to look at. This works in most cases, but it can be tedious to do for every single file, especially for dataset with dozens or hundreds of scans.

Another approach is to use Amazon Web Servces (aws) by using their `Command Line Interface <https://aws.amazon.com/cli/>`__; this requires downloading their "aws" command and then using it to download the entire dataset. After installing the command, you can use it to download an entire dataset in one go, which is more efficient than downloading each subject or file manually.

Yet another approach which combines the advantages of both of the above is called `datalad <https://handbook.datalad.org/en/latest/index.html>`__, a data management tool that allows you the flexibility to download specific files using the command line. 
