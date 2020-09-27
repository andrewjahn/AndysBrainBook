.. _ML_06_Haxby_Scripting:

=======================================
Machine Learning Tutorial #6: Scripting
=======================================

---------------

Overview
********

[Typical stuff about why scripting is good]


Editing the Script
******************

We will first make a copy of the script ``Haxby_Script_job.m`` by saving it as ``Haxby_Script_allSubjs.m`` and saving it in the ``Haxby_Data`` folder. Then add this line of code near the top of the script:

::

  subjects = [2 3 4 6];
  
  for subject=subjects
  
and remember to add the string ``end`` to the very last line of the script to close the for-loop.

Now replace ``pwd '/sub-1`` with ``pwd filesep subject '``


Change [ pwd '/SPM_Results_sub-1' ] to [ pwd '/SPM_Results_' subject]
