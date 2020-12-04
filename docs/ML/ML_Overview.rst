.. ML_Overview:

=================================
Machine Learning for Neuroimagers
=================================

--------------

Overview
********


.. figure:: Haxby_Fig3A.png

Machine Learning is a method of using data to train a classifier; this is called **training data**. The classifier is then provided with new data (also known as **testing data**), and it attempts to distinguish between different classes within the data based on the training data. The classifier's performance is judged by its accuracy - how many of the testing data points it managed to correctly classify.

After working through an example with AFNI to learn the basics, we will begin to use a Matlab package called `The Decoding Toolbox <https://sites.google.com/site/tdtdecodingtoolbox/>`__. This will expand our options of different classifiers to use, such as searchlight algorithms and representational similarity analysis (RSA). To begin reading an overview of machine learning and how it is used with fMRI data, click the ``Next`` button; otherwise, select any of the chapters below to begin using either 3dsvm or The Decoding Toolbox.

.. toctree::
   :maxdepth: 1
   :caption: Introduction to Machine Learning

   ML_Short_Course/ML_00_Introduction
   ML_Short_Course/ML_01_Brown_Example
   ML_Short_Course/ML_02_Haxby_Intro_Download
   ML_Short_Course/ML_03_Haxby_Preprocessing
   ML_Short_Course/ML_04_Haxby_Timing
   ML_Short_Course/ML_05_Haxby_MVPA
   ML_Short_Course/ML_06_Haxby_Scripting
   ML_Short_Course/ML_07_Haxby_GroupAnalysis
   ML_Short_Course/ML_AppendixA_AFNI_Code
