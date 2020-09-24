.. _ML_04_Haxby_Timing:

=========================
Creating the Timing Files
=========================

-------------

Overview
********

Each subject in the dataset has 12 timing files in their ``func`` folder, corresponding to each run of functional data. For example, the first few lines of the file ``sub-1_task_objectviewing_run-01_events.tsv`` in the folder ``sub-1/func`` looks like this:

::

  onset   duration        trial_type
  12.000  0.500   scissors
  14.000  0.500   scissors
  16.000  0.500   scissors
  18.000  0.500   scissors
  20.000  0.500   scissors
  22.000  0.500   scissors
  24.000  0.500   scissors
  26.000  0.500   scissors
  28.000  0.500   scissors
  30.000  0.500   scissors
  32.000  0.500   scissors
  34.000  0.500   scissors
  48.000  0.500   face
  50.000  0.500   face
  52.000  0.500   face
  54.000  0.500   face
  56.000  0.500   face
  58.000  0.500   face
  60.000  0.500   face
  62.000  0.500   face
  64.000  0.500   face
  66.000  0.500   face
  68.000  0.500   face
  70.000  0.500   face
  
According to the Haxby et al. 2001 paper, each condition lasted for 24 seconds, and this is reflected in the Duration field that we specified in the last chapter. We will therefore need to extract the first onset time for each condition for each run, and concatenate them into a single timing file per condition. Later, we will use this timing file to fill in the remaining fields of our script.

The following code will extract the first onset time for each condition automatically, looping over all of the subjects in the dataset. You can either copy and paste this into the terminal, first making sure you are in the directory ``Haxby_Data`` that contains all of the subjects; else, you can copy it into a file, name it ``make_Timings.sh``, and run it by typing ``bash make_Timings.sh``:

::

  #!/bin/bash
  for subj in sub-1 sub-2 sub-3 sub-4 sub-5 sub-6; do
    cd ${subj}/func
      for cond in bottle cat chair face house scissors scrambledpix shoe; do
        if [ -f "$cond.txt" ]; then
          rm ${cond}.txt
        fi
            for i in `seq -w 1 12`; do
                    cat ${subj}_task-objectviewing_run-${i}_events.tsv | awk -v i="$cond" '{if ($3==i) print $1}' | head -1 >> ${cond}.txt
            done
      done
    cd ../..
  done
  
This will generate files labeled ``bottle.txt``, ``cat.txt``, and so on, one for each condition, and place it in the appropriate subject's ``func`` folder. The contents of sub-1's ``bottle.txt`` file, for example, will look like this:

::

  228.000
  192.000
  156.000
  228.000
  84.000
  228.000
  264.000
  156.000
  228.000
  264.000
  120.000
  12.000
  
Note that these onset times are relative to the start of each run. For example, the first value of 228.000 means that the "bottle" condition occurred 228 seconds into the first run; the next value indicates that 192.000 seconds into the second run was when the "bottle" condition was next presented; and so on.


Modifying the Script
********************


