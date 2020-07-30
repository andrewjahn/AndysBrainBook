.. _AppendixA_LGI:

====================================
Appendix A: Local Gyrification Index
====================================

.. note::

  This section is under construction.
  
  
First, you need to set the Path to point to your Matlab. In my case, I typed:

::

  export PATH=/Applications/MATLAB_R2017a.app/bin:${PATH}
  
Type ``getmatlab`` to make sure the path has been set properly.

To run the LGI analysis, type:

::

  recon-all -s <subj> -localGI
  
This will take a couple of hours. New files will be created with an ``_lgi`` extension.
