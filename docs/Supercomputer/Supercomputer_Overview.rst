.. _Supercomputer/Supercomputer_Overview:

==============================
Introduction to Supercomputing
==============================

--------------

This is a brief overview of how to use a supercomputing cluster, in particular the University of Michigan's :ref:`Great Lakes cluster <https://arc.umich.edu/greatlakes/>`. We will also review how to submit jobs to the Open Science Grid (OSG), which is an open-access supercomputer used for many different purposes; we will learn how to use it to submit a large number of FreeSurfer analysis jobs. This tutorial will focus on how to use a supercomputer for analyzing neuroimaging data, and how to optimize the resources used for your analysis.

.. note::

  There is an excellent introduction to supercomputing and the batch submission language, slurm, written by Bennet Fauber `here <https://justbennet.github.io/umich-cluster-neuroimaging/>`__. This tutorial will cover much of the same ground written on that website.
  
  
Benefits of Supercomputing
**************************

In previous tutorials about analyzing neuroimaging data, we discussed how you can save time by **scripting** your analyses; in other words, automating each analysis so that it can be run in the background of your computer. As long as the organization of your data is standardized (e.g., in `BIDS format <https://bids.neuroimaging.io/>`__), with minor edits the scripts can also be used to analyze other datasets.

In the previous tutorials, however, it was assumed that you were using a single computer to analyze the data. You may not have enough computing resources to both run the analyses in the background and do other work at the same time, or you may quickly run out of space to store the data, limiting how many subject you can analyze at a time. External hard drives may be a temporary solution, but even then, with large datasets you can run out of space quickly.

Supercomputing clusters get around these problems by both providing much larger storage space than the average computer, and by providing many processors that can be used simultaneously to analyze a large dataset. Researchers using a hundred processors at a time can finish an analysis an order or magnitude faster than someone using a single computer with eight processors.
