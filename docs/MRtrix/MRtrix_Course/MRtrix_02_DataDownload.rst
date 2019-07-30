.. _MRtrix_02_DataDownload:

=========================================
MRtrix Tutorial #2: Downloading the Dataset
=========================================

--------------

For this course, we will be analyzing a dataset from openneuro.org called `BTC preop <https://openneuro.org/datasets/ds001226/versions/00001>`__. It includes data from patients with gliomas, patients with meningiomas, and a group of control subjects. We will be comparing the groups against each other, as well as running correlation analyses with the covariates included in the dataset's ``participants.tsv`` file.

To download the data, click on `this link <https://openneuro.org/datasets/ds001226/versions/00001>`__ and then click on the ``Download`` button. 

.. figure:: 02_Download_BTC.png


When the download has finished, unzip the folder, and then rename it to BTC_preop:

::

  mv ds001226-000001 BTC_preop
  
  
You will then be ready to begin looking at the data in the next chapter.
