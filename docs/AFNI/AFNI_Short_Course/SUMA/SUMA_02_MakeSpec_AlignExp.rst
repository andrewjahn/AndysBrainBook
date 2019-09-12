.. _SUMA_02_MakeSpec_AlignExp:

==============================================================
SUMA Tutorial #2: @SUMA_MakeSpecFS and @SUMA_AlignToExperiment
==============================================================

-----------------

Overview
**********

Once you have reconstructed the cortical surfaces, you can use SUMA commands to align those surfaces to the original anatomical image. The command @SUMA_MakeSpecFS will create a new surface file that can be read by SUMA, and @SUMA_AlignToExperiment will register the surface to your anatomical dataset. Once you have the output from these commands, you can use them with the ``afni_proc.py`` command to create a surface-based analysis script.
