.. _Github_Overview:

==================
Overview of Github
==================

.. note:: This page was created for the 2021 University of Michigan fMRI Course. More material will be added to it as time goes on.

------------------

Introduction
************

**Github** is a website for hosting code: Python, C++, Matlab, and any other computer language. Code is hosted in **repositories** created by a user, and this code can be copied to a local machine (in Github terminology, **cloned**). 


Glossary
********

The following is a list of terms that you will come across with Github. Example code has been provided for certain terms to build your intuition about what these words mean.

* Repository: A webpage that contains folders and sub-folders to help organize a project. For example, Andy's github (found `here <https://github.com/andrewjahn?tab=repositories>__`) contains repositories such as ``AFNI_Scripts``, ``SPM_Scripts``, and ``FSL_Scripts``. Each of these folders in turn contains code that is relevant for analyzing data using AFNI, SPM, or FSL, respectively.

.. figure:: Github_Repositories.png

  Example of repositories on a Github page.
  
* Clone: Copying a repository to your local machine. For example, if I want to clone the repository ``SPM_Scripts`` from Andy's Github page, I would need to know the link to the page (i.e., https://github.com/andrewjahn/SPM_Scripts), and then use it with the ``git`` command:

..

  git clone https://github.com/andrewjahn/SPM_Scripts
  
This will clone the SPM_Scripts repository to my local machine, from where I ran the ``git`` command.
