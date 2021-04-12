.. _PM_01_DataDownload:

=======================
Downloading the Dataset
=======================

-------------

As with the datasets for the tutorials in SPM, FSL, and AFNI, we will download our data from openneuro.org. Click on `this link <https://openneuro.org/datasets/ds000005/versions/00001>`__ to see the Mixed Gambles dataset.

.. figure:: 01_GamblesPage.png

Download the dataset by clicking on the "Download" button at the top of the page. The dataset is about 2 Gigabytes, and comes in a zipped folder. Extract it by double-clicking on the folder.


After you have downloaded and unzipped the dataset, click on the Next button for an overview of the experimental task used in this study.

Alternative Download Options
****************************

If the download button doesn't work, try using the `Amazon Web Services (AWS) <https://aws.amazon.com/>`__ option. Go to `this page <https://aws.amazon.com/cli/>`__ and download the appropriate AWS client for your operating system. Once it has been installed, open a Terminal, navigate to the Desktop, and type the following:

::

    aws s3 sync --no-sign-request s3://openneuro.org/ds000005 ds000005-download/

It should take about half an hour to download.
