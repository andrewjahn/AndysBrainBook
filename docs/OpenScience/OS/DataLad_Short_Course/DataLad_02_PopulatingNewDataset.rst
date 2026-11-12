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

