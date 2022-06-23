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

For our purposes, however, we will be focusing on **Open On-Demand**, a graphical user interface that emulates a personal computer through your web browser. Click on ``My Interactive Sessions``, and from the list, click on ``Basic Desktop``. In the field ``Slurm account``, enter the account name that was registered for you when you applied for a UMRCP; in my case, it is ``ajahn0``. For ``Partition``, select ``standard``. The rest of the fields - numer of cores, memory, and number of GPUs - do not matter that much for our Basic Desktop, since most of the analyses we will be doing will be submitted as ``batch jobs``, explained in more detail below. The main use for the Basic Desktop is to download data, use data viewers such as ``AFNI`` to ``fsleyes``, and to provide an environment that is easier to navigate than a single shell. For now, I will set the ``Number of hours`` to ``120``, the ``Number of cores`` to ``1``, and ``Memory`` to ``8``. We won't need GPUs for this Desktop, so we will leave it at ``0``.

.. figure:: 05_InteractiveApps.png

When you have filled out all of these fields, click on the ``Launch`` button, which will submit a request for a new Basic Desktop session. When it has loaded, click on ``Launch Basic Desktop``. This will open a new tab in your web browser that will look like a standard Desktop. On the menu bar at the bottom, there is an icon for a Terminal, and an icon for a web browser. Click on the Terminal icon to open a new Terminal, and click on the web browser icon to open a Firefox web browser:

.. figure:: 05_Desktop.png

To run CAT12 on the computing cluster, we will have to download both SPM12 and CAT12 to our home directory, just like we did in the SPM12 tutorial and an earlier chapter of this CAT12 tutorial. Follow those instructions to install both packages in your home directory, and to install the CAT12 program in the ``spm12/toolbox`` directory.

Once you have installed the software, return to the Great Lakes dashboard, and from the ``Interactive Apps`` list, select ``MATLAB``. Similar to the Basic Desktop Interactive App, we need to specify the computational resources we require. Select the ``MATLAB version`` ``2020a``, 120 hours, 4 cores, and 10GB of memory. We will be using this app just to create a batch template, which will then be submitted via batch jobs on the computing cluster. Click the ``Launch`` button, and then ``Launch MATLAB`` when it is ready.

When Matlab has appeared in a new tab, type ``addpath ~/spm12`` from the terminal, assuming that you installed SPM12 in your home directory. Then type ``spm fmri`` to open the program. From the dropdown menu underneath ``toolbox``, select ``cat12``. Click the ``Segment`` button. 
