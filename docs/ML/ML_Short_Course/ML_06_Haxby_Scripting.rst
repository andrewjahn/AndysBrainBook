.. _ML_06_Haxby_Scripting:

=======================================
Machine Learning Tutorial #6: Scripting
=======================================

---------------

Overview
********

In a :ref:`previous chapter <ML_04_Haxby_Timing>` in this module, you edited a script that was generated through the SPM GUI. We will use that same script and impose a for-loop on it in order to analyze all of the subjects, one after another, without our having to do anything in between. This makes it much easier and less tedious to analyze large numbers of subjects, especially when they are all formatted identically with the same number of runs and timing files - the only difference is the subject number, which will be changed on each iteration of the loop.


Editing the Script
******************

We will first make a copy of the script ``Haxby_Script_job.m`` by saving it as ``Haxby_Script_allSubjs.m`` and saving it in the ``Haxby_Data`` folder. Then add this line of code near the top of the script:

::

  subjects = [2 3 4 6];
  
  for subject=subjects
  
and remember to add the string ``end`` to the very last line of the script to close the for-loop.

.. note::

  Subject 5 has 11 runs instead of the usual 12 runs that all of the other subjects have, so we will save that for another script below.

To make this script work for any subject, we will need to replace any occurrences of ``sub-1`` with the variable located in ``subject``. Open the Find and Replace menu, and replace every instance of ``pwd '/sub-1/func/sub-1`` with ``pwd '/sub-' subject '/func/sub-' subject '``


Change [ pwd '/SPM_Results_sub-1' ] to [ pwd '/SPM_Results_' subject]
