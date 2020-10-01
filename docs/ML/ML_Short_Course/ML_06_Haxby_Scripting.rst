.. _ML_06_Haxby_Scripting:

=======================================
Machine Learning Tutorial #6: Scripting
=======================================

---------------

Overview
********

In a :ref:`previous chapter <ML_04_Haxby_Timing>` in this module, you edited a script that was generated through the SPM GUI. We will use that same script and impose a for-loop on it in order to analyze all of the subjects, one after another, without our having to do anything in between. This makes it much easier and less tedious to analyze large numbers of subjects, especially when they are all formatted identically with the same number of runs and timing files - the only difference is the subject number, which will be changed on each iteration of the loop.


Editing the Preprocessing Script
********************************

We will first make a copy of the script ``Haxby_Script_job.m`` by saving it as ``Haxby_Script_allSubjs.m`` and saving it in the ``Haxby_Data`` folder. Then add this line of code near the top of the script:

::

  subjects = [2 3 4 6];
  
  for subject=subjects
  
and remember to add the string ``end`` to the very last line of the script to close the for-loop.

.. note::

  Subject 5 has 11 runs instead of the usual 12 runs that all of the other subjects have, so we will save that for another script below.

To make this script work for any subject, we will need to replace any occurrences of ``sub-1`` with the variable located in ``subject``. Open the Find and Replace menu, and replace every instance of ``pwd '/sub-1/func/sub-1`` with ``pwd '/sub-' subject '/func/sub-' subject '``. Also change ``[ pwd '/SPM_Results_sub-1' ]`` to ``[ pwd '/SPM_Results_' subject]``.

In the section that loads the onset times, you will need to change each condition from something like this:

::

  load('sub-1/func/bottle.txt');
  
to this:

::

  load(['sub-' subject '/func/bottle.txt']);
  
  Repeat for all of the other conditions.

  Lastly, near the beginning of the script, just after the for loops is defined, insert the following code:

  ::

    subject = num2str(subject);
  % Check whether the files have been unzipped. If not, unzip them using
  % gunzip

  if exist([ pwd filesep 'sub-' subject '/anat/sub-' subject '_T1w.nii' ]) == 0
      display('Anatomical image has not been unzipped; unzipping now')
      gunzip([ pwd filesep 'sub-' subject '/anat/sub-' subject '_T1w.nii.gz' ])
  else
      display('Anatomical image is already unzipped; doing nothing')
  end

  runs = [ 01 02 03 04 05 06 07 08 09 10 11 12 ];

  for run=runs

  run = num2str(run, '%02d'); % Zero-pads each number so that the "run" variable is 2 characters long    

  if exist([ pwd filesep 'sub-' subject '/func/sub-' subject '_task-objectviewing_run-' run '_bold.nii']) == 0
      display([ 'Run ' run ' has not been unzipped; unzipping now'])
      gunzip([ pwd filesep 'sub-' subject '/func/sub-' subject '_task-objectviewing_run-' run '_bold.nii.gz' ])
  else
      display(['Run ' run ' is already unzipped; doing nothing'])
  end

  end
  
This will check whether the files are zipped, and if so, it will unzip them. This is necessary for them to be loaded into the SPM batch structure.


.. note::

  It might be easier to download a script that has already been edited, and compare it against the script you generated in the previous chapters. Click `here <https://github.com/andrewjahn/MachineLearning>`__ and download the file "Haxby_Script_allSubjs.m". If you place it in the directory ``Haxby_Data``, you should be able to run it from the terminal by typing ``Haxby_Script_allSubjs`` and pressing ``Enter``.
  
When the script has finished running for subjects 2, 3, 4, and 6, the only subject remaining is number 5. Change the ``subjects`` array from ``[2 3 4 6]`` to ``[5]``, and remove the number ``12`` from the ``runs`` array. Then comment out the line ``{[ pwd filesep 'sub-' subject '/func/sub-' subject '_task-objectviewing_run-12_bold.nii']}``, and any line of code that begins with ``matlabbatch{4}.spm.stats.fmri_spec.sess(12)``. Run the script again, saving it as a separate file if you want to.

Editing the MVPA Scripts
************************

The changes to the MVPA scripts are similar to the edits for the preprocessing. At the beginning of the script we will declare our for-loop:

::

  for subject=subjects
    
  subject = num2str(subject);
  
And then change the code for setting the results and beta maps directories:

::

  % Set the output directory where data will be saved, e.g. 'c:\exp\results\buttonpress'
  cfg.results.dir = [pwd '/SPM_Results_' subject];

  % Set the filepath where your SPM.mat and all related betas are, e.g. 'c:\exp\glm\model_button'
  beta_loc = [pwd '/SPM_Results_' subject];
  
And run the script from the terminal. As an exercise, when it has finished modify the script again to do a searchlight analysis for all of the subjects, using the methods you learned in the last chapter. A template script can be downloaded `here <https://github.com/andrewjahn/MachineLearning>`__, under the file ``Haxby_MVPA_Scripted``.

Next Steps
**********

The ROI results may be all that you need for your analysis; with an accuracy value per condition for each subject, these can be used as values in a t-test. Keep in mind that they need to be compared to chance, as opposed to a baseline of zero. (This might be why one of the outputs you can select is accuracy minus chance; that removes the need for an additional step of subtracting chance.)

If you are instead interested in the searchlight whole-brain results, on the other hand, we will need to normalize them to MNI space. To see how to do that, click the ``Next`` button.
