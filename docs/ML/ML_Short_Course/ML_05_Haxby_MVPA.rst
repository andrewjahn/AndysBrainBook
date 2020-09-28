.. _ML_05_Haxby_MVPA:

=====================================================================
Machine Learning Tutorial #5: MVPA Analysis with The Decoding Toolbox
=====================================================================

-----------

The Decoding Toolbox
********************

Having generated beta maps for this subject - one per condition for each run - we are now ready to run a **multi-variate pattern analysis**, using the beta maps as both training and testing data. This is similar to what we did in the first tutorial with AFNI's ``3dsvm`` command, but we will be using a different package called **The Decoding Toolbox**.

A software package that runs in Matlab, The Decoding Toolbox can be downloaded from its homepage, located `here <https://sites.google.com/site/tdtdecodingtoolbox/>`__. Scroll down the page and click on the link ``Click here to download TDT``. Unzip the file when it is finished downloading, and move it to your home directory. Then open a Matlab terminal, click on "Set Path" at the top of the ``Home`` tab, and then click on ``Add Folder``. Select the directory ``decoding_toolbox`` within the folder ``tdt_3.999``, then click ``Open`` and ``Save``.

.. figure:: 05_TDT_Path.png


Modifying the Template Script
*****************************

The Decoding Toolbox contains many different commands and scripts, which I encourage you to explore at your leisure. For the current study, however, you will only need to modify one script: ``decoding_template.m``. Open this file by typing ``open decoding_template`` from the command line, which will display the file in the editor window. Using the ``Editor`` tab, click on ``Save -> Save Copy As...`` and save it to the folder ``Haxby_Data``, labeling it ``Haxby_MVPA_ROI``.

You will see several fields that need to be filled in or commented out. The first couple of ``addpath`` commands can be used to set the path to The Decoding Toolbox and SPM, if you need to; since we have already set those paths, we can comment out these lines. 
