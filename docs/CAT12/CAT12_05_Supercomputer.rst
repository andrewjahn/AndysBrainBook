.. _CAT12_05_Supercomputer:

================================================
CAT12 Tutorial #5: Analysis on the Supercomputer
================================================

------------------

Motivation for Using the Supercomputing Cluster
***********************************************

As you saw in the previous tutorial, CAT12 has an option for processing many subjects in separate **processing jobs**. We used four processors, which divided the group of twelve subjects we had into four jobs with three subjects each. Although this meant that those processors were unavailable for other programs on my computer, it did increase the speed of the CAT12 analysis by several times. This is a much more efficient method than serial processing, which would analyze one subject at a time, not beginning the next one until the current subject is completed.

We can extend this **parallel processing** method to take advantage of the hundreds of processors available on a typical supercomputing cluster. I will be using `Great Lakes <https://arc.umich.edu/greatlakes/>`__, the supercomputing cluster for the University of Michigan.

Overview of Great Lakes
***********************

For most University of Michigan graduate students and researchers affiliated with the department of Literature, Science, and the Arts (LSA), you are able to apply for a free supercomputing account through Michigan's Advanced Research Computing (ARC) center. Usually, a new supercomputing account requires a principal investigator to provide a shortcode that charges their grant or other funds; however, for those who will use only a modest amount of computing resources, you can apply for a University of `Michigan Research Computing Package (UMRCP) <https://arc.umich.edu/UMRCP/>`__. This package provides you with 80,000 CPU hours and 10TB of storage per year, along with 100TB of archive storage which can also be requested. These resources should work for most purposes, and we will be using it for the rest of the tutorial.

You can apply for an account on the `UMRCP webpage <https://arc.umich.edu/UMRCP/>`__ and clicking on ``Sign up for the UMRCP``. You will need to provide your title, affiliation, University ID, and a brief description of the research you will be doing on the cluster. You will also need to specify whether the data is sensitive or not; see a list `here <https://safecomputing.umich.edu/dataguide/?q=node/246>`__ of data that is allowed to be analyzed on the cluster.

Using Open On-Demand
********************

Once your account has been approved, use `this link <https://greatlakes.arc-ts.umich.edu/pun/sys/dashboard/>`__ to access the Great Lakes dashboard. You will need to log in with your username and password, and use Duo authentication as well. Once you are logged in, you should see something like this:

.. figure:: 05_GreatLakes_Dashboard.png

Click on ``Files -> Home Directory`` to see the files and folders located in your home directory. This is the same data organization structure as what you would see in a Unix terminal, or in your Desktop GUI. The ``Upload`` and ``Download`` buttons allow you to upload and download files, respectively, which can be useful if you need to transfer a shell script from your personal computer to the cluster, or to download fully preprocessed results onto your local machine. If you feel more comfortable using a shell, you can click on the button ``Open in Terminal`` to use a Unix terminal.

For our purposes, however, we will be focusing on **Open On-Demand**, a graphical user interface that emulates a personal computer through your web browser. Click on ``My Interactive Sessions``, and from the list, click on ``Basic Desktop``. In the field ``Slurm account``, enter the account name that was registered for you when you applied for a UMRCP; in my case, it is ``ajahn0``. For ``Partition``, select ``standard``. The rest of the fields - number of cores, memory, and number of GPUs - do not matter that much for our Basic Desktop, since most of the analyses we will be doing will be submitted as ``batch jobs``, explained in more detail below. The main use for the Basic Desktop is to download data, use data viewers such as ``AFNI`` to ``fsleyes``, and to provide an environment that is easier to navigate than a single shell. For now, I will set the ``Number of hours`` to ``120``, the ``Number of cores`` to ``1``, and ``Memory`` to ``8``. We won't need GPUs for this Desktop, so we will leave it at ``0``.

.. figure:: 05_InteractiveApps.png

When you have filled out all of these fields, click on the ``Launch`` button, which will submit a request for a new Basic Desktop session. When it has loaded, click on ``Launch Basic Desktop``. This will open a new tab in your web browser that will look like a standard Desktop. On the menu bar at the bottom, there is an icon for a Terminal, and an icon for a web browser. Click on the Terminal icon to open a new Terminal, and click on the web browser icon to open a Firefox web browser:

.. figure:: 05_Desktop.png

To run CAT12 on the computing cluster, we will have to download both SPM12 and CAT12 to our home directory, just like we did in the SPM12 tutorial and an earlier chapter of this CAT12 tutorial. Follow those instructions to install both packages in your home directory, and to install the CAT12 program in the ``spm12/toolbox`` directory. Likewise, navigate to the ADNI website, go to Data Collections, and click on ``ADN1:Baseline 3T``. Check the box next to ``All`` to select all of the images, and then select ``1-Click Download`` and click on ``Zip File 1``. Once the images have been downloaded, open a Terminal, type ``cd ~/Downloads``, and then type ``unzip ADNI1\ Baseline\ 3T.zip``. This will expand all of the files into a new directory called ``ADNI``. We will move this to our storage directory ``turbo`` by typing: ``mv ADNI /nfs/turbo/lsa-ajahn``.

To organize the data and make it more manageable, use the following code to create a file, subjList.txt, that contains a list of all the subject IDs; it will then rename each subject's anatomical image to a more compact label including the subject ID, and then remove the subdirectories that are no longer needed:

::

  #!/bin/bash
  
  if [ ! -f subjList.txt ]; then
    ls | grep _S_ > subjList.txt
  fi
  
  for i in `cat subjList.txt`; do
    mv $i/MPR*Scaled/*/*/*.nii $i/${i}_T1w.nii
    rm -r $i/MPR*
  done
  
Each subject will then have their own anatomical image:

.. figure:: 05_Anatomical_Images.png


Creating a Template Script
**************************

Once you have installed the software and downloaded the data, return to the Great Lakes dashboard. From the ``Interactive Apps`` list, select ``MATLAB``. Similar to the Basic Desktop Interactive App, we need to specify the computational resources we require. Select the ``MATLAB version`` ``2020a``, 120 hours, 4 cores, and 10GB of memory. We will be using this app just to create a batch template, which will then be submitted via batch jobs on the computing cluster. Click the ``Launch`` button, and then ``Launch MATLAB`` when it is ready.

When Matlab has appeared in a new tab, navigate to the directory where the anatomical images are stored by typing ``cd /nfs/turbo/lsa-ajahn``. Then, type ``addpath ~/spm12`` from the terminal, assuming that you installed SPM12 in your home directory, and navigate to our tutorial data by typing ``cd /nfs/turbo/lsa-ajahn/ADNI``. Type ``spm fmri`` to open the program, and from the dropdown menu underneath ``toolbox``, select ``cat12``. Click the ``Segment`` button. Similar to what we did on the personal computer, we just have to enter the volumes that need to be segmented. However, since we will be generating an individual batch for each subject, we should change the ``Split job into separate processes`` from the default of ``4`` to ``0``, assigning each job to a single processor.

Double-click on ``Volumes``, navigate to the folder ``002_S_0413``, and select the file ``002_S_0413_T1w.nii``. There is nothing special about this file; we just need a single file to create our template. Click on ``File -> Save Batch and Script``, and name the file ``CAT12_SPM_Batch_Template``, saving it to the directory that contains all of the subjects. Use a text editor to open the file ``CAT12_SPM_Batch_Template_job.m``, and change the subject ID - in this case, ``002_S_0413`` - to ``changeme``. This string of text will be used later in a ``sed`` command to generate a batch file for each subject. Also, at the very end of the file, add this line of code:

::

  spm_jobman('run', matlabbatch)
  
Which will run all of the code above it stored in the ``matlabbatch`` structure.

Next, we will create a template file for the Matlab commands to be run from the terminal. Although we created the batch template file using Matlab, we will be submitting all of the jobs from the command line, and can run them using Matlab commands but without opening the Matlab GUI. Use a text editor to create a file called ``Matlab_Template_Job.sh``, and enter the following code into it:

::

  #!/bin/bash -l
  '/sw/arcts/centos7/matlab/R2020a/bin/matlab' -nodesktop -noFigureWindows -nosplash -r "addpath '~/spm12'; changeme; exit"
  echo "Job finished for subject changeme without errors"
  
The options ``-nodesktop``, ``noFigureWindows``, and ``-nosplash`` signal that the Matlab GUI should not be opened up, and everything after the ``-r`` option is normal Matlab code, similar to what you would run if you were in a Matlab terminal. Our goal is to 
