.. _FS_05_OpenScienceGrid:

.. warning::

  fsurf has been deprecated and is no longer supported; the Open Science Grid will no longer be creating accounts for new users. I have kept the following tutorial available in order to illustrate how jobs are submitted to a supercomputer - this may be superseded by a tutorial about using another supercomputer, such as Michigan's "Great Lakes" supercomputer.

===================================================
FreeSurfer Tutorial #5: Using the Open Science Grid
===================================================

-----------

Time Constraints with Recon-All
*******************************

Even if you are able to run multiple jobs with the parallel command, it may not be practical for very large datasets - for example, a study that includes hundreds of subjects. You also may not want to have all of your processing cores tied up in running recon-all, and would prefer to have your computer free to use for other projects.

One option is to use a **supercomputer**, which is available at most universities. If you don't have access to one, then you can use a publicly available supercomputer hosted by the `Open Science Grid <https://opensciencegrid.org/>`__, which uses processing cores on computers located across over a hundred sites - laboratories, universities, and other institutions. You can submit a recon-all command to the Open Science Grid, which is then distributed to one of the many cores that are available. For most imaging researchers there is essentially no limit on how many jobs you can submit; a batch containing one or two hundred jobs isn't very large by supercomputer standards, and the entire batch can usually be finished in less than a week.


Preparing Your Data for the Open Science Grid
*********************************************

Before you can use any of the Open Science Grid resources, you must create an account `here <https://support.opensciencegrid.org/support/solutions/articles/12000008488-set-up-fsurf-on-your-laptop>`__.

You will also need a command called ``fsurf`` to submit recon-all jobs to the Open Science Grid supercomputer. To download this command, type:

::

  curl -L -o fsurf 'http://stash.osgconnect.net/+fsurf/fsurf'
  chmod +x fsurf
  
And then move the fsurf executible into a directory that your PATH points to. For example, most operating systems have a path that by default points to the ``/bin`` directory - the same directory that contains commands such as ``ls``, ``cd``, and ``pwd``. If you move ``fsurf`` to ``/bin``, then you can run the command from any directory:

::

  sudo mv fsurf /bin
  
.. note::

  In the code example above, ``sudo`` is used to move fsurf to the /bin directory. That is because the /bin directory is considered sensitive - you don't want anyone changing it unless they know what they're doing. Consequently, sudo will prompt you to enter your password before it moves the file.
  

Next, create a list of all of the subjects by typing the following code:

::

  ls | grep sub- > subjList.txt
  
This will pipe the results of the ``ls`` command into a file called subjList.txt. We will then use this list to create a for-loop to submit all of our recon-all jobs to the Open Science Grid supercomputer.


Submitting Recon-All Jobs
*************************

The Open Science Grid is particular about how the jobs are submitted; each anatomical image needs to be packaged in a certain way, just as you need to package items when you drop them off at the post office. 

First you will need to run recon-all on your anatomical images, omitting the ``-all`` option. This will create a series of directories, and then convert the anatomical image to .mgz format and place it in the ``mri/orig`` directory. The following code can be either copied and pasted into the Terminal, or you can copy it into a shell script and run it with ``tcsh``:

::

  foreach subj (`cat subjList.txt`)
        cd $subj/ses-BL/anat
        if (! -d $subj ) then #If the FS directory doesn't exist, then run recon-all
                recon-all -s $subj -i *.nii.gz -sd .
                #zip the FreeSurfer directories, so they can be submitted to fsurf
                zip -r $subj.zip $subj
                cd ../../..
        else
                echo "FreeSurfer folder for $subj already exists; if you want to rerun recon-all for this subject, delete the folder and rerun this script."
                cd ../../..
        endif
    end


Once that has finished, you can submit the jobs using ``fsurf``. In this example, I've placed ``fsurf`` in a for-loop:

::

  foreach subj (`cat subjList.txt`)
        cd $subj/ses-BL/anat
        fsurf submit --subject=$subj --input=$subj.zip --defaced --deidentified --version 6.0.0 --freesurfer-options='-all -qcache -3T'
        cd ../../..
  end

The status of the jobs can then be checked by typing ``fsurf list``, which will print several columns to the screen. The first column is the subject name, the second column is the subject ID assigned by the Open Science Grid supercomputer, and the second-to-last column specifies whether the job is running, completed, or has failed. Periodically check the status of these jobs to see which ones can be downloaded.


.. note::

  The previous code examples are written in ``tcsh`` instead of ``bash``. You can write it in either one; I just happened to be using ``tcsh`` at the time.


Downloading or Removing Jobs
****************************

Once recon-all has finished, you can download the output by typing this code:

::

  fsurf output --id <subjID>
  
In which ``subjID`` is the identifcation code assigned by the supercomputer. It is the number in the second column of the output of the command ``fsurf list``. The downloaded data will have . ``.bz2`` extension; you can unpack it by typing ``tar xvjf <subjName>``, replacing ``subjName`` with the name of the downloaded dataset.


On the other hand, if you want to remove a job at any time for any reason, you can do so by typing:

::

  fsurf remove --id <subjID>
  
``subjID`` is found the same way as above.


--------

.. Video
.. ********

.. To see how to download fsurf and run jobs on the Open Science Grid supercomputer, watch `this video <https://www.youtube.com/watch?v=30eIVOgr35A&list=PLIQIswOrUH6_DWy5mJlSfj6AWY0y9iUce&index=5>`__.
