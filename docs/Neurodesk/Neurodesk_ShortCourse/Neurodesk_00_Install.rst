.. _Neurodesk_00_Install:

.. _AFNI_Overview:

===========================================
Neurodesk Tutorial #1: Download and Install
===========================================

---------------

Downloading the Right Version
*****************************

Neurodesk comes in two flavors: One that you can access directly from a web browser, and a downloadable version that is run as a container. The benefits of using Neurodesk within a web browser is that it is quick, easy, and doesn't require installing anything on your local machine. The downsides are that you are limited in the amount of data you can upload and the amount of memory you can use, and that your instance will be deleted after about twenty-four hours. Also, it is not as secure as analyzing data on your own machine, so you probably don't want to upload any data that is HIPAA protected or not deidentified.

That said, this chapter will focus on downloading the container version. I am using a Macintosh computer with a Silicon processor (i.e., Apple M1 Silicon chip), but you will have to download the version that is correct for your machine. Neurodesk should work regardless of what operating system or computer you are using, whether it be Macintosh, Windows, or Unix.

Docker version 4.32.0: https://docs.docker.com/desktop/release-notes/#4320
NeurodeskApp: https://www.neurodesk.org/docs/getting-started/neurodesktop/neurodeskapp/ 

.. note::

  As of this writing (October 2024), it appears that Docker versions 4.33 and above do not work with Macintosh computers with Apple Silicon chips; this may change in the future, but version 4.32.0 does work. This version will be available until January 4th, 2025.

You will need to select the version appropriate for your computer. For example, if you have a Macbook Pro with an M1 chip, you would download the "Mac with Apple chip" version of Docker, and the "macOS Apple silicon Installer" for the NeurodeskApp.

Between now and the workshop, follow the installation instructions for each of those packages, and then follow the instructions on the NeurodeskApp site about creating a Local Connection. If everything goes well, you should see something like this:

.. figure:: Neurodesk_Launcher.png

If you click on the Neurodesktop launcher, it will open a new window. To test whether the libraries work, open a Terminal by clicking the icon in the bottom tray of the new window. Then, in the terminal that opens, type "ml afni" and "3dinfo"; you should see something like this:

.. figure:: Neurodesk_Setup.png
