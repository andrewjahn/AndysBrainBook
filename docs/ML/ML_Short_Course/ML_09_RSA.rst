.. _ML_09_RSA:

==================================================================
Machine Learning Tutorial #9: Representational Similarity Analysis
==================================================================

---------------

Overview
********

Representational Similarity Analysis (RSA) 


Downloading the Data
********************

The dataset for this analysis can be found `here <http://www.bccn-berlin.de/tdt/downloads/sub01_firstlevel.zip>`__, on The Decoding Toolbox's website. This folder contains the fully preprocessed and analyzed data for a single subject, with betas generated for each trial of each condition. After you have downloaded the dataset, move it to your Desktop directory, and look inside:

.. figure:: 09_sub01_Directory.png

The folder ``sub01_GLM`` contains the betas estimated from the GLM; the folder ``sub01_ROI`` contains different ROIs that can be used for the classifications analysis, such as the left primary motor cortex and primary visual cortex; and ``sub01_struct`` contains the anatomical image. For this tutorial, we will be using the data in the GLM and ROI folders.

Creating the Script
*******************

The Decoding Toolbox has a template RSA script lcoated within ``tdt_3.999F/decoding_toolbox/templates``; if your path already points to this folder, you can navigate to the folder ``sub01_firstlevel``, and then type ``open decoding_template_similarity.m``. Save a copy of it to the ``sub01_firstlevel`` folder, and call it ``RSA_SampleScript.m``. Open this new file in your Matlab terminal by typing ``RSA_SampleScript.m``, and close the original template script.

