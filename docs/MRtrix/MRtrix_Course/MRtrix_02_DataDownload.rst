.. _MRtrix_02_DataDownload:

=========================================
MRtrix Tutorial #2: Downloading the Dataset
=========================================

--------------

For this course, we will be analyzing a dataset from openneuro.org called `BTC preop <https://openneuro.org/datasets/ds001226/versions/00001>`__. It includes data from patients with gliomas, patients with meningiomas, and a group of control subjects. We will be comparing the groups against each other, as well as running correlation analyses with the covariates included in the dataset's ``participants.tsv`` file.

To download the data, click on `this link <https://openneuro.org/datasets/ds001226/versions/00001>`__ and then click on the ``Download`` button. 

.. figure:: 02_Download_BTC.png


When the download has finished, unzip the folder, open a Terminal, and then rename it to BTC_preop:

::

  mv ~/Downloads/ds001226-00001 ~/Desktop/BTC_preop
  
This assumes that the dataset was put in the "Downloads" directory. The command will place the renamed directory on your Desktop.
  
.. note::

  If you don't have the space for all of the data, you can start with the data for just one subject. Click on the ``sub-CON02`` folder to expand the list of contents, and download each file separately. Then create a the following subfolders in your BTC_preop directory by navigating to that directory and typing ``mkdir -p sub-CON02/ses-preop/anat sub-CON02/ses-preop/dwi sub-CON02/ses-preop/func``. Then move the images you download to their corresponding directory; i.e., the anatomical images will go in the anat folder, the diffusion images will go in the dwi folder, and so on.
  
  
You will then be ready to begin looking at the data in the next chapter.


Video
*****

Click `here <https://www.youtube.com/watch?v=DSnuxQZx7zk>`__ for a walkthrough of how to download the dataset.
