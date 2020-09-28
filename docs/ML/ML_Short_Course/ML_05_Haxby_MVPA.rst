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

On line 17, we have different analysis options. The default, **searchlight**, will analyze all of the voxels within a mask (or the entire brain) and determine which voxels are most useful for decoding a particular condition. We will revisit this in a later tutorial; for now, change this string to "ROI", which will calculate the decoding accuracy across all of the voxels for each condition within a particular mask.

Lines 21 and 24, ``cfg.results.dir`` and ``beta_loc``, can both be set to ``[pwd '/SPM_Results_1']``.

On line 31, we will set a path to the mask. In the original Haxby paper, this mask was created by manually selecting the voxels within the lingual,
fusiform, inferior occipital, and middle occipital gyri, and then running an omnibus test to detect any voxels that responded to any of the conditions, thresholded at a p-value of p=0.000001. Any significant voxels within the manually defined mask were then binarized to create a new mask.

We could do this ourselves, but the masks have already been provided by the authors on `this website <http://data.pymvpa.org/datasets/haxby2001/>`__. Clicking on the file **subj1-2010.01.14.tar.gz**, for example, will download the original functional and timing data for subj1, including the mask used in the original paper. Download this folder and then unzip it, and note that there are several masks that you are able to choose from; the file ``mask4_vt.nii.gz`` is a ventral temporal mask that contains the significant voxels from the omnibus test. Open a terminal, navigate to the directory ``subj1`` that you just downloaded, and unzip the file by typing ``gunzip mask4_vt.nii.gz``; then navigate to the ``Haxby_Data`` directory, create a new directory for the masks by typing ``mkdir Haxby_Masks``, and move the file to this folder (e.g., ``movefile('~/Downloads/subj1/mask4_vt.nii', '~/Desktop/Haxby_Data/Haxby_Masks/mask4_vt_1.nii')``).

Beginning on line 36, we can list all of the conditions we have in our experiment. Since there were 8, we will create 8 labelnames, one for each condition; e.g.,

::

  labelname1 = 'bottle';
  labelname2 = 'cat';
  labelname3 = 'chair';
  labelname4 = 'face';
  labelname5 = 'house';
  labelname6 = 'scissors';
  labelname7 = 'scrambledpix';
  labelname8 = 'shoe';
  
So far, the modified script should look something like this:

.. figure:: 05_Haxby_Script_Edited1.png

The last line to edit is near the end, which starts with ``cfg=decoding_describe_data``. This contains only two conditions; to expand it to our 8 conditions, we will replace it with this code:

::

  cfg = decoding_describe_data(cfg,{labelname1 labelname2 labelname3 labelname4 labelname5 labelname6 labelname7 labelname8},[1 2 3 4 5 6 7 8],regressor_names,beta_loc);
  
Now save the file and run it from the terminal by typing ``Haxby_MVPA_ROI``.
