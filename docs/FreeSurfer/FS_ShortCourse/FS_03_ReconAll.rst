.. _FS_03_ReconAll:

============
FreeSurfer Tutorial #3: Recon-all
============

Overview: Reconstructing the Cortical Surface
*********

FreeSurfer contains a large suite of programs which can take several hours to process a single subject, and days to process an entire dataset. This fact alone may seem daunting; but FreeSurfer has a single command that does virtually all of the preprocessing. This command, **recon-all** is simple to use and takes only a few arguments. Later on, you will learn how to run multiple recon-all commands simultaneously, which will save you a considerable amount of time.

Recon-all stands for **reconstruction**, as in reconstructing a two-dimensional cortical surface from a three-dimensional volume. The images we collect from an MRI scanner are three-dimensional blocks, and these images are transformed by recon-all into a smooth, continuous, two-dimensional surface - similar to crumpling up a paper lunch bag into the size of a pellet, and then blowing into it to expand it like a balloon.

.. figure:: 03_Reconstruction.png

  The command recon-all converts a three-dimensional anatomical volume (shown on the left, represented by a typical sagittal slice taken through a volume) into a two-dimensional surface (right). As you will see in the tutorial on Freeview, FreeSurfer creates several different types of inflated brains that you can use for visualizing your results.
  
.. This may seem a counterintuitive way to learn about how a command works, but allow me to explain.   


The Output of Recon-all
*********

Before discussing how to use the recon-all command, you should be acquainted with what it creates. Recon-all first strips the skull from the anatomical image to generate a dataset called **brainmask.mgz**. (The .mgz extension stands for a compressed, or zipped, Massachusetts General Hospital file; it is an extension specific to FreeSurfer, similar to AFNI's BRIK/HEAD extension.) Recon-all then makes an estimate of where the interface is between the white matter and grey matter (stored in a file called **orig.mgz**, which stands for **orig**inal estimate).

This initial estimate is refined and stored in a file called **white.mgz**. This boundary is used as a base from which recon-all extends feelers to search for the edge of the grey matter. Once this edge is reached, a third dataset is created: **pial.mgz**. This dataset represents the pial surface, which is like a thin, translucent plastic film wrapped around the edge of the grey matter. Each one of these datasets can be viewed as a surface.

.. figure:: 03_Orig_White_Pial.png

  An illustration of how recon-all creates different surfaces. The original estimate of the location of the white-matter and grey-matter interface (yellow) is refined into a more accurate estimate (blue). This refined estimate is then used to assist with detecting the edge of the grey matter (red). These surfaces as seen in Freeview (FreeSurfer's viewer) are shown on the right.

One of the advantages of using these surfaces is the ability to depict within the sulci measurements such as cortical thickness differences or BOLD signal. In a three-dimensional volume, it is possible for a single voxel to span two separate ridges of grey matter, which makes it impossible to determine which part of the cortex generates the observed signal. The surfaces created by FreeSurfer can be further expanded to create the datasets **lh.inflated** and **rh.inflated**. 

.. figure:: 03_Pial_Inflated.png

  An illustration of converting the lh.pial file into lh.inflated.
  
These inflated surfaces can be inflated yet again, this time into a sphere. This is not an image that you use to visualize results; it is an image that is instead normalized to a template image called **fsaverage**, which is an average of 40 subjects. Once the individual subject's surface map is normalized to this template, an atlas can be used to **parcellate** the brain into anatomically distinct regions. Recon-all will parcellate the individual subject's brain according to two atlases: The Desikan-Killiany atlas, and the Destrieux atlas. The Destrieux atlas contains more parcellations; which one you end up using for your analysis will depend on how fine-grained an analysis you wish to carry out.

.. figure:: 03_FreeSurfer_Atlases.png

  A comparison of the Desikan-Killiany (left) and Destrieux (right) atlases.


Using the Recon-all command
********

The recon-all command requires a T1-weighted anatomical image with good contrast between the white matter and the grey matter. If your anatomical image is called subj1_anat.nii, for example, you can run recon-all with the following command:

::

  recon-all -s subj1 -i subj1_anat.nii -all

Navigate to where your anatomical images are located. In this example, there are six anatomical scans from different subjects. If you want to process just one of them, type the following: 

.. note::

  If you get a permission error, type the following:
  Sudo chmod -R a+w $SUBJECTS_DIR
  And then rerun the recon-all 
  
I also recommend adding the qcache option, which will smooth the data at different levels and store them in the subject’s output directory. These will be useful for group level analyses, which we will cover in a future tutorial. If you’ve already run the recon-all preprocessing on your subjects, you can run qcache with the following command:

::

  Recon-all -s <subjectName> -qcache
  
Which should take about 10 minutes per subject.


---------

Video
**********

For a video overview of recon-all and how to use it, click `here <https://www.youtube.com/watch?v=gkjvKMjH7iM>`__.
