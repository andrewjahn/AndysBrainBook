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

It should take about half an hour to download. When it finishes downloading, move it to the Desktop and rename it by opening a Terminal and typing:

::

    mv ~/Downloads/ds000005-download ~/Desktop/Gambles
    
Analyzing the Dataset in Different Software Packages
****************************************************

When you have downloaded the data, you have a choice of analyzing it in any of the major fMRI analysis packages:

1. SPM (LINK)
2. FSL (LINK)
3. AFNI (LINK)

You can use any of these packages to do parametric modulation, although the details are slightly different between them. If you are already familiar with how to preprocessing a typical dataset using these packages, the major difference will be in setting up the general linear model to estimate regressors for the parametric modulators.


Video
*****

For a video on how parametric modualtion works, click here (INSERT LINK).
