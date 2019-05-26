.. _Unix_09_AutomatingTheAnalysis:

Unix Tutorial #9: Automating The Analysis
==================

----------------

Overview
*********

As we begin this tutorial, we have fully merged with the associated :ref:`fMRI course <fMRI_01_DataDownload>`. Most of the text here is borrowed from the fMRI chapter on :ref:`scripting <fMRI_06_Scripting>`, which uses all of the commands we have learned. The following section will show you how to integrate conditionals, for-loops, and sed in order to integrate separate lines of code into a useful script.

Running the Script
**********

The code below is designed to edit and run a file that analyzes each subject in the Flanker dataset. Once you have `created the template <https://andysbrainbook.readthedocs.io/en/latest/fMRI_Short_Course/fMRI_06_Scripting.html#creating-the-template>`__, move the design_run1.fsf and design_run2.fsf files to the directory containing your subjects (i.e., ``mv design*.fsf ..``, and then ``cd ..``). Then download the script `run_1stLevel_Analysis.sh <https://github.com/andrewjahn/FSL_Scripts/blob/master/run_1stLevel_Analysis.sh>`__ and move it to the Flanker directory. The script is reprinted here:

::

  #!/bin/bash

  # Generate the subject list to make modifying this script
  # to run just a subset of subjects easier.

  for id in `seq -w 1 26` ; do
      subj="sub-$id"
      echo "===> Starting processing of $subj"
      echo
      cd $subj

          # If the brain mask doesn’t exist, create it
          if [ ! -f anat/${subj}_T1w_brain_f02.nii.gz ]; then
              bet2 anat/${subj}_T1w.nii.gz \
                  echo "Skull-stripped brain not found, using bet with a fractional intensity threshold of 0.2" \
                  anat/${subj}_T1w_brain_f02.nii.gz -f 0.2 #Note: This fractional intensity appears to work well for most of the subjects in the Flanker dataset. You may want to change it if you modify this script for your own study.
          fi

          # Copy the design files into the subject directory, and then
          # change “sub-08” to the current subject number
          cp ../design_run1.fsf .
          cp ../design_run2.fsf .

          # Note that we are using the | character to delimit the patterns
          # instead of the usual / character because there are / characters
          # in the pattern.
          sed -i '' "s|sub-08|${subj}|g" \
              design_run1.fsf
          sed -i '' "s|sub-08|${subj}|g" \
              design_run2.fsf

          # Now everything is set up to run feat
          echo "===> Starting feat for run 1"
          feat design_run1.fsf
          echo "===> Starting feat for run 2"
          feat design_run2.fsf
                  echo

      # Go back to the directory containing all of the subjects, and repeat the loop
      cd ..
  done

  echo

Analyzing the Script
**********

Let's walk through each part of this script and describe what it does. 

Initializing the for-loop
^^^^^^^^^^

It begins with a shebang and some comments describing what exactly the script does; and then backticks are used to expand ``seq -w 1 26`` in order to create a loop that will run the body of the code over all of the subjects. This will expand to ``01, 02, 03 ... 26`` and update the number that is assigned to the variable ``id`` on each iteration of the loop.

::

  #!/bin/bash

  # Generate the subject list to make modifying this script
  # to run just a subset of subjects easier.

  for id in `seq -w 1 26` ; do
      subj="sub-$id"
      echo "===> Starting processing of $subj"
      echo
      cd $subj


For example, the first loop of this code will assign the string ``sub-01`` to the variable ``subj``, then echo "===> Starting processing of sub-01". It will then navigate into the ``sub-01`` directory.


Conditionals to check for the skull-stripped anatomical
^^^^^^^^^^

The script then uses a conditional to check whether the skull-stripped anatomical exists, and if it doesn't, the skull-stripped image is generated. 

::

          # If the brain mask doesn’t exist, create it
          if [ ! -f anat/${subj}_T1w_brain_f02.nii.gz ]; then
              bet2 anat/${subj}_T1w.nii.gz \
                  echo "Skull-stripped brain not found, using bet with a fractional intensity threshold of 0.2" \
                  anat/${subj}_T1w_brain_f02.nii.gz -f 0.2 #Note: This fractional intensity appears to work well for most of the subjects in the Flanker dataset. You may want to change it if you modify this script for your own study.
          fi
      
      
Editing and running the template file
^^^^^^^^^^

Then the template design*.fsf file is edited to replace the string ``sub-08`` with the current subject's name. The *.fsf files are run with the command ``feat``, which is like running the FEAT GUI from the command line. Echo commands are used throughout the script to let the user know when a new step is being run.

::

          # Copy the design files into the subject directory, and then
          # change “sub-08” to the current subject number
          cp ../design_run1.fsf .
          cp ../design_run2.fsf .

          # Note that we are using the | character to delimit the patterns
          # instead of the usual / character because there are / characters
          # in the pattern.
          sed -i '' "s|sub-08|${subj}|g" \
              design_run1.fsf
          sed -i '' "s|sub-08|${subj}|g" \
              design_run2.fsf
              
           
The design.fsf files, which are located in the main Flanker directory, are copied into the current subject's directory. Sed then replaces the string ``sub-08`` with the current value of ``subj`` that has been assigned in the loop. The last part of the code runs the .fsf files with the ``feat`` command, and prints to the Terminal which run is being analyzed.

::

          # Now everything is set up to run feat
          echo "===> Starting feat for run 1"
          feat design_run1.fsf
          echo "===> Starting feat for run 2"
          feat design_run2.fsf
                  echo
                  
                  
You can run the script by simply typing ``bash run_1stLevel_Analysis.sh``. The echo commands will print text to the Terminal when a new step is run, and HTML pages will track the progress of the preprocessing and statistics.


Summary
***********

At this point you have learned all the necessary Unix commands and concepts to run an fMRI analysis script. If this is your first time using Unix, this may seem complicated; but with practice, you will be able to see why the script is composed the way it is, and how in relatively few lines is able to represent what can take dozens of hours of human labor. 

By investing the time to learn Unix now, you will be able to make your analyses quicker, more efficient, and less prone to error. You will also, I hope, have become more confident in taking the first steps toward applying your new skills to writing analysis script of your own. 
