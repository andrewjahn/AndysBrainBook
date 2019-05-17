.. _Unix_06_IfElse.rst

.. note

  Topics covered: conditionals, if/else statements
  
  Commands covered: if/else, -e, ! -e, elif
  
.. note

  5.17.2019 Right now this is just a transcription of the video screenplay; will be removing some of these examples from the video, and keeping them here instead.

=============
Unix Tutorial #6: Conditional Statements
=============

At this point, we’ll start to use our Unix commands with fMRI data. Click on the link in the More Info box down below for instructions about how to download the Flanker dataset, install FSL, and what skull-stripping is. When you’ve finished, come back to this tutorial.

The previous video on for-loops showed us how to run many blocks of code with slight alterations between each execution. But what if we only want to run the code if certain conditions are met? We can automate these decisions with **conditional statements**, also called if-else statements: IF a certain condition is true, THEN do something; else if the condition is not true, do something else.

For example, we might want to check the anatomical directory for whether the anatomical image has been skull-stripped. If it hasn’t been skull-stripped, then do the skull-stripping; if it’s already been stripped, then do nothing.

Navigate into the sub-01 anatomical directory. From the command line, type:

::

  if [[ -e sub-01_T1w_brain_f02.nii.gz ]]; then
  	echo “Skull-stripped brain exists”
  fi

Like the for-loops, an if-else statement has three distinct sections. The first section begins with the word “if”, and then evaluates, or checks, whether the statement in the brackets is true or false. Within the brackets, the -e stands for “check whether this file exists.” If anat_brain.nii.gz exists, then the statement goes on to run the code in the body of the conditional statement. You can have as many lines of code in the body as you want. The last line, fi - or “if” spelled backwards - ends the conditional, and then proceeds to run any code listed afterwards.

NOTE: the format of the if-else statement needs to be exact: You need exactly one space between the first bracket and the -e, for example. If it’s not formatted like this, you will get an error.

Note that if you typed out the conditional statement above, nothing happened. That’s because in this case the statement was evaluated as FALSE: That file doesn’t exist, so the code isn’t run. If we want to do something if the conditional statement is false, we need to add another section: An ELSE section, which looks like this:

::

if [[ -e sub-01_T1w_brain_f02.nii.gz ]]; then
	echo “Skull-stripped brain exists”
else
	echo “Skull-stripped brain does not exist”
fi

This means that if this conditional statement is true, then run this block of code (highlight). If it isn’t true, then run this block of code (highlight).

You can use multiple conditionals to give your if/else statement more flexibility. For example, let’s say we want to check whether the skull-stripped image exists. If it doesn’t, evaluate whether the original anatomical image exists. If that doesn’t exist either, then print that neither the skull-stripped nor the original anatomical image exists. This requires an elif statement, which stands for “else, if”. 

::

if [[ -e sub-01_T1w_brain_f02.nii.gz ]]; then
	echo “Skull-stripped brain exists”
elif [[ -e sub-01_T1w.nii.gz ]]; then
	echo “Original anatomical brain exists”
else
	echo “Neither the skull-stripped nor the original brain exists”
fi

You can use other so-called logical expressions to evaluate whether statements are true or false. For example, within the brackets you can use a pair of ampersands to check whether both files exist:

::

if [[ -e sub-01_T1w.nii.gz && -e sub-01_T1w_f02_brain.nii.gz ]]; then
	echo “Both files exist”
else
	echo “One or more files do not exist”
fi

Or you can use a pair of vertical pipes to check whether one file OR the other exists:

::

if [[ -e sub-01_T1w.nii.gz || -e sub-01_T1w_f02_brain.nii.gz ]]; then
	echo “At least one of the files exists”
else
	echo “Neither of the files exists”
fi

You can also check if a file DOESN’T exist by placing an exclamation mark before the -e option:

::

if [[ ! -e sub-01_T1w_f02_brain.nii.gz ]]; then
	echo “The skull-stripped brain doesn’t exist”
else
	echo “The skull-stripped brain does exist”
fi

For now, we will end with a demonstration of how to combine a for-loop with an if/else statement. Let’s say that we want to check whether subjects 1, 2, and 3 have a skull-stripped anatomical image. If it doesn’t exist, strip the skull using bet2. Navigate to the directory containing all of your subjects, and then run the following code:

::

for i in sub-01 sub-02 sub-03; do
	cd ${i}/anat
	if [[ ! -e ${i}_T1w_f02_brain.nii.gz ]]; then
		echo “Skull-stripped brain doesn’t exist; stripping the brain with a fractional intensity threshold of 0.2”
		bet2 ${i}_T1w.nii.gz ${i}_T1w_f02_brain.nii.gz -f 0.2
	else
		echo “Skull-Stripped brain already exists; doing nothing”
	fi
	cd ../..
done

This will navigate into each subject’s anatomical directory and check whether the skull-stripped image exists. If it doesn’t, then run bet to skull-strip the anatomical. The echo commands are optional; I like to include them so that the user knows what command is currently being run.

We covered a lot of concepts in this tutorial, but time and practice will make you more familiar with how to integrate for-loops and conditional statements into your code. The next tutorial will show you how to write all of these commands into a script, which makes your code more portable and easier to edit.


----------


Exercises
*******



--------


Video
********
