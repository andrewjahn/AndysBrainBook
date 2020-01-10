.. _SPM_Intermezzo_Toolboxes:


=========================
SPM Intermezzo: Toolboxes
=========================

---------

.. note::

  The full set of available toolboxes for SPM can be found `here <https://www.fil.ion.ucl.ac.uk/spm/ext/>`__.

Overview
********

Although SPM comes with an extensive library for analyzing fMRI data, it doesn't have some of the tools you will need for more advanced analyses, such as the **region of interest** analyses covered in the next chapter. To meet this need, programmers and researchers have created SPM **extensions** (also known as **toolboxes**) which enable the user to do specific analyses.

For example, most fMRI researchers use some kind of **atlas**, or partitioning of the brain into distinct functional or anatomical regions. fMRI data can then be extracted from a specified partition, or region of interest, and then statistics are performed to determine whether there is a significan effect in that region. To create these anatomical regions of interest, we will download and install the **WFU Pickatlas** toolbox, a popular atlas and region of interest generator.

To download this toolbox, click on `this link <https://www.nitrc.org/projects/wfu_pickatlas/>`__, and then click the Download button - an arrow pointing downwards, just underneath the selection menu. If you do not already have an account with the NITRC (Neuroimaging Tools and Research Collaboratory, a repository for code and toolboxes), you will need to create one. Agree to the terms, and proceed with the download. 

When the toolbox has been downloaded, unzip it, and type the following code:

::

  movefile ~/Downloads/WFU_PickAtlas_3.0.5b/* ~/spm12/toolbox
  
This will move all of the needed folders into the SPM12 toolbox folder, where they will be read each time you open SPM.

Now open SPM, click on the ``Toolbox`` menu, and you should see the wfupickatlas toolbox as an option.


Installing Marsbar
******************

Marsbar is another popular SPM toolbox, used primarily for ROI analysis. To download it, click on `this link <https://sourceforge.net/projects/marsbar/files/>`__ and click the Download button.

After the package has been downloaded, unzip it. From the Matlab terminal, navigate to the spm12 toolbox directory and create a directory called marsbar:

::

  cd ~/spm12/toolbox
  mkdir marsbar
  
And then move the required files from the marsbar download into the marsbar folder:

::

  movefile ~/Downloads/marsbar-0.44/* ~/spm12/toolbox/marsbar
  
The next time you open SPM, you should see "marsbar" as an option when you click on the Toolbox menu.


Next Steps
**********

After you have installed the WFU PickAtlas and Marsbar toolboxes, you are ready to do a region of interest analysis, which we turn to next.
