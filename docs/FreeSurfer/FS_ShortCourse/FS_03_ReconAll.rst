.. _FS_03_ReconAll:

============
FreeSurfer Tutorial #3: Recon-all
============

Overview
*********

FreeSurfer is a very complex program, which can take several hours to process a single subject, and days to process an entire dataset. At the same time, many of the commands are very simple to use. This video will review the command recon-all, which does all of the FreeSurfer preprocessing.
Recon-all stands for reconstruction, as in reconstructing a cortical surface (show volume being converted into a surface). All it requires is a T1-weighted anatomical image with good contrast between the white matter and the grey matter (highlight the grey matter and white matter on a T1-weighted image).
Navigate to where your anatomical images are located. In this example, there are six anatomical scans from different subjects. If you want to process just one of them, type the following: recon-all -s subj1 -i subj1_anat.nii -all
If you get a permission error, type the following:
Sudo chmod -R a+w $SUBJECTS_DIR
And then rerun the recon-all 
I also recommend adding the qcache option, which will smooth the data at different levels and store them in the subject’s output directory. These will be useful for group level analyses, which we will cover in a future tutorial. If you’ve already run the recon-all preprocessing on your subjects, you can run qcache with the following command:
Recon-all -s <subjectName> -qcache
Which should take about 10 minutes per subject
Can run recon-all with parallel [maybe a separate tutorial for this?]
NB: You will need to be in the “bash” shell for this to work!
Sample command: ls *.nii | parallel --jobs [# processors] recon-all -s {.} -i {} -all -qcache
