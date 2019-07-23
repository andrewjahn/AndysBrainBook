.. _FS_07_FSGD:

=====================================
FreeSurfer Tutorial #7: The FSGD File
=====================================

---------------

Comparing Groups
*****************

So far we have reviewed basic commands of FreeSurfer: recon-all and freeview. Recon-all creates a series of volumes and surfaces from a T1-weighted anatomical image, and quantifies the amount of grey matter thickness and volume in different regions of the brain. The cortical regions are called **parcellations**, and the sub-cortical regions are called **segmentations**. For example, the left superior frontal gyrus is a parcellation delineated by the Desikan-Killiany atlas, and structural measurements are calculated in that region for each subject. The right amygdala, on the other hand, is a segmentation; because the sub-cortical regions are not inflated into a surface, they only contain measurements of grey matter volume, not thickness.

At this point, you may be thinking about how to compare structural measurements between groups, and represent those differences on the surface of the brain. The procedure is similar to fMRI analysis: Just as we compare voxels in fMRI, we compare vertices in FreeSurfer. If the vertices are in a common space such as MNI, we can calculate the difference in grey matter thickness at a particular vertex and test whether that difference is significant. This generates statistical maps that we can overlay on a template brain as a surface map.

.. figure:: Group_Analysis_FS.png

  Example of a group analysis result mapped on to the fsaverage template. Lighter blue colors represent more negative contrast estimates, and yellow colors represent more positive contrast estimates.
  
  
The template brain that we use, which was mentioned in passing in the earlier tutorials, is called ``fsaverage``. It is an average of 40 subjects that were combined using the spherical averaging technique described in `Fischl et al., 1999 <https://tinyurl.com/y4ubdg78>`__. The coordinates of this brain are used as a standardized space for the rest of the brains which are normalized to the fsaverage template.


Organizing the Directories
**********

First, we will copy the fsaverage template into our current directory. Navigate to the Cannabis directory which contains all of the subjects, and then type:

::

  cp -R $FREESURFER_HOME/subjects/fsaverage .
  
to copy the fsaverage template. When that has completed, set the SUBJECTS_DIR variable to the current directory. This will place the output of any recon-all or group analysis commands into the current directory:

::

  setenv SUBJECTS_DIR `pwd`
  
.. note::

  The backticks which bookend ``pwd`` will expand the current directory path; in other words, it will replace ```pwd``` with whatever the output is of typing the command ``pwd``. This kind of shorthand will be useful, so practice it whenever you have the chance.
  
We will also create two directories called ``FSGD`` and ``Contrasts``, which will contain the text files needed to run our analysis:

::

  mkdir FSGD Contrasts
  

Creating the FSGD File
**********

The Cannabis dataset comes with a file called ``participants.tsv`` that contains labels and covariates for each subject: Group, gender, age, onset of cannabis use, and so on. To create a FreeSurfer Group Descriptor (FSGD) file, we will extract those covariates or group labels that we are interested in and format them in a way that FreeSurfer understands. The FSGD file will both contain the covariates that we want to contrast, and a separate contrast file will indicate which covariates to contrast, and which weight to give them.

.. figure:: Participant_File.png

  Screenshot of part of the participant file that comes with the Cannabis dataset. Note: HC = Healthy Controls. Farther down the participant file, the label CB stands for Cannabis User.
  
::

  cp ../particpants.tsv FSGD/CannabisStudy.tsv.
  
  
------------


Video
**********

For a video demonstration of how to create the FSGD file, click `here <https://www.youtube.com/watch?v=3T9PuME2g9A&list=PLIQIswOrUH6_DWy5mJlSfj6AWY0y9iUce&index=7>`__.
