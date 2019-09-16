.. _SUMA_02_MakeSpec_AlignExp:

==============================================================
SUMA Tutorial #2: @SUMA_Make_Spec_FS and @SUMA_AlignToExperiment
==============================================================

-----------------

Overview
**********

Once you have reconstructed the cortical surfaces, you can use SUMA commands to align those surfaces to the original anatomical image. The command @SUMA_Make_Spec_FS will create a new surface file that can be read by SUMA, and @SUMA_AlignToExperiment will register the surface to your anatomical dataset. Once you have the output from these commands, you can use them with the ``afni_proc.py`` command to create a surface-based analysis script.

SUMA_Make_Spec_FS
*****************

The command ``@SUMA_Make_Spec_FS`` can either be run directly in the ``surf`` directory created by FreeSurfer's recon-all command, or you can specify the path using the ``-fspath`` option. In either case, in the ``surf`` directory a new directory called ``SUMA`` will be created.

::

  FLANKER_DIR=/Users/$USER/Desktop/Flanker
  FS_DIR=${FLANKER_DIR}/FS

  for subj in `cat subjList.txt`; do
         @SUMA_Make_Spec_FS -sid ${subj} -fspath ${FS_DIR}/${subj}_T1w/surf
  done
  
This will take around 20-30 minutes per subject.


SUMA_AlignToExperiment
**********************
