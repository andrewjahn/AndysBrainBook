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
*************************

Once your account has been approved, use `this link <https://greatlakes.arc-ts.umich.edu/pun/sys/dashboard/>`__ to access the Great Lakes dashboard. You will need to log in with your username and password, and use Duo authentication as well. Once you are logged in, you should see something like this:

.. figure:: 05_GreatLakes_Dashboard.png

Click on ``Files -> Home Directory`` to see the files and folders located in your home directory. This is the same data organization structure as what you would see in a Unix terminal, or in your Desktop GUI. The ``Upload`` and ``Download`` buttons allow you to upload and download files, respectively, which can be useful if you need to transfer a shell script from your personal computer to the cluster, or to download fully preprocessed results onto your local machine. If you feel more comfortable using a shell, you can click on the button ``Open in Terminal`` to use a Unix terminal.

For our purposes, however, we will be focusing on **Open On-Demand**, a graphical user interface that emulates a personal computer through your web browser. Click on ``My Interactive Sessions``, and 
