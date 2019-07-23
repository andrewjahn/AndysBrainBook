.. _fASL_02_Download:

=================
Downloading f-ASL
=================

-----------

Overview
********

.. note::
    You will need to have Matlab installed to run f-ASL. Most universities can give you a license for free; ask your local IT person for more information.

f-ASL (functional ASL) is an ASL analysis package written by `Luis Hernandez-Garcia <http://web.eecs.umich.edu/~hernan/>`__. The package is available for download from Luis's github page located `here <https://github.com/HernandezGarciaLab>`__. Click on the ``Matlab_ASL_Repo`` repository, and click on the ``Clone or Download`` button; select ``Download ZIP``. Once the package has been downloaded, open up a Terminal and navigate to your Downloads directory, and type:

::

    unzip Matlab_ASL_repo-master.zip
    
This will create a folder called ``Matlab_ASL_Repo-master``, which you can then move to your home directory by typing:

::

    mv Matlab_ASL_repo-master ~
    

.. note::

    If you need a review of Unix and how to use basic Terminal commands, see the :doc:`Unix tutorial <https://andysbrainbook.readthedocs.io/en/latest/unix/Unix_Intro.html>`__.
    


---------

Setting the Path
****************

In order to use f-ASL from anywhere on your computer, you will need to set a path pointing to the f-ASL libraries. If LuisTools is in your Downloads directory, for example, open up Matlab and then type:

::
    
    addpath ~/Matlab_ASL_repo-master
    
    
You will then be able to open up f-ASL by typing ``fasl02`` and pressing Enter. The following GIF shows how to both download the package and how to set the path:

.. figure:: 02_fASL_Download_Install.gif


.. note::
    
    In order to run fASL from any directory, you will need to type the ``addpath`` command above each time you open a new instance of Matlab. An alternative is to open a Matlab window, then click on ``Set Path`` and then click on the ``Add with Subfolders`` button. Select the ``Matlab_ASL_repo-master`` directory, then click ``Save`` and ``Close``. Now you will be able to open fASL any time you open up a new Matlab window, without having to type the ``addpath`` command first.
    
    
    
---------

Video
*****

Click here to see a video of how to download and install f-ASL (coming soon).


Next Steps
**********

Now that you have f-ASL installed, we will use it to analyze a sample ASL dataset. Click the ``Next`` button to learn more about the dataset that we will analyze.
