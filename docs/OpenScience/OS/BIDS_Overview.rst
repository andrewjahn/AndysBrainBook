.. _BIDS_Overview:

=============
BIDS Overview and Tutorial
=============

-------------

.. note::

  This article was contributed by `Daniel Levitas <https://perceptionandneuroimaging.psych.indiana.edu/people/daniellevitas.html>`__ of the Perception and Neuroimaging Lab at Indiana University.
  
.. note::

  The Stanford Center for Reproducibility (which created BIDS) has their own tutorial `here <http://reproducibility.stanford.edu/bids-tutorial-series-part-1a/>`__.

What is BIDS?
*************


BIDS (Brain Imaging Data Structure) is a standarized format for the organization and description of neuroimaging and corresponding behavioral data, which has been largely lacking within the neuroimaging community. More specifically, data that come off the scanner are converted to NIFTI and JSON files, organized into a specific directory schema, and labeled following a precise naming convention. The result is an organized dataset that can be easily shared and understood by other researchers.

Benefits of BIDS
****************

1. Reproducibility and Data Sharing: One of the more common issues that can arise for imaging researchers is receiving data from a colleague and not being able to make heads or tails of it: how the data are named and organized do not make intuitive sense to you, finding pertinent information (e.g. Repetition Time) is time-consuming, etc. What transpires is time wasted trying to make sense of the data, leading to needless back and forth with colleague(s). Thanks to the standarized format of BIDS however, the hassles of data sharing are largely alleviated. By extension, improved data sharing allows researchers to better assess and reproduce others’ experimental findings.

2. Access to “BIDS-apps”: Once converted to BIDS format, data can be applied to software packages that take BIDS-formatted datasets as their input, colloquially referred to as “BIDS-apps”. Two of the most common and used apps are MRIQC (a quality control pipeline that generates metrics of your data), and fMRIPrep (a standarized pre-processing pipeline). Having access to fMRIPrep is incredibly useful, as labs (and even individuals within a single lab) typically have their own idiosyncratic pre-processing pipelines, which can influence findings in subsequent analyses, and create confusion for others when attempting to re-process the data. By having a standarized pipeline such as fMRIPrep, which incorporates different sofware packages (such as FSL, AFNI, ANTs, FreeSurfer, and nipype), datasets across labs/institutions can be pre-processed in a highly reproducible and transparent manner.

3. Share your own BIDS-app(s): It is not uncommon for researchers to develop a novel analysis tool or technique that languishes on their Github or some other repository, unable to gain exposure in the neuroimaging community. As BIDS becomes increasingly popular however, dataset organization and pre-processing is becoming more standardized, which can be used by various BIDS-apps. By developing a tool that accepts BIDS-formatted data, you have created a BIDS-app that is potentially applicable to a large range of users.


Okay, I’m sold. How do I convert my data to BIDS?
**********************************************

You will need to have access to your or others' raw data (e.g. dicoms) in order to perform the conversion. There are several different packages that can be used to accomplish this, such as `dcm2bids <https://github.com/cbedetti/Dcm2Bids>`__, `heudiconv <https://github.com/nipy/heudiconv>`__, `bidscoin <https://github.com/Donders-Institute/bidscoin>`__, `bidskit <https://github.com/jmtyszka/bidskit>`__, etc. Admittingly, most BIDS converters require a bit of user input and work, therefore this section includes an in-depth tutorial to provide guidance throughout the process.

BIDS Tutorial
*************

For this tutorial we will be using the dcm2bids converter; however, as you become familiarized with BIDS you may find a different converter that suits your fancy.

Before diving into the tutorial, we first need to get the data, set up a project folder for this tutorial, and install some software packages.

Create Project folder
*********************

Throughout this tutorial we will be using the command line, so you will need to be working in a MacOS or Linux environment. To access the command line you will need to open your terminal. Once open, type the following, line by line:

::

  cd $HOME
  mkdir BIDS_tutorial
  
You have now created our project folder (BIDS_tutorial) in your home directory.
  
Download data
*************
The data can be found `here <https://drive.google.com/drive/folders/1vVW3jPfYdvh52juiHTUnkmY6bjvxZQcR?usp=sharing>`__. Be sure to download the entire folder and not individual files. Due to the size of the folder, it will take several minutes to zip and download everything. If the download appears to have stalled, cancel the current download and retry. Once downloaded, extract/unzip the files (in your Downloads folder), and type the following into the command line, line by line:

::

  mv ~/Downloads/BIDS_tutorial_data $HOME/BIDS_tutorial
  
The data has now been moved into your project folder

CMake and pip Installation
**************************

Depending on your computer or server, you may already have CMake and/or pip; however, this is no guarantee. To determine which you have these installed and in your $PATH variable, type the following in the command line, line by line:

::

  which cmake
  which pip
  
If these packages are in your $PATH, it should list where they're located, otherwise you will need to install them. To install CMake, go `here <https://cmake.org/download/>`__ and click the tar.gz file for your MacOS or Linux. Then type the following into your command line, line by line:

::

  cd ~/Downloads
  tar -zxvf cmake-3.16.3-*-x86_64.tar.gz
  export PATH="~/Downloads/cmake-3.16.3-Darwin-x86_64/CMake.app/Contents/bin/:$PATH"


To install pip (if for some reason you don't have it), type the following into the command line, line by line:

::

  cd
  curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
  python get-pip.py --user

You will see a warning message that lists where pip is installed. Copy that path and add it to your $PATH. An example of how you do that is:

::

  export PATH="/Users/dlevitas/Library/Python/2.7/bin/:$PATH"
  
  
At this point, CMake and pip should now be installed and in your $PATH


dcm2niix Installation
*********************

dcm2niix is the standard for converting raw data to NIFTI/JSON file format. It is extremely versatile and can be used on raw data from different scanners (Siemens, Phillips, GE). To install the package on your machine or HPC account, type the following into your command line, line by line:

::

  cd ~
  git clone https://github.com/rordenlab/dcm2niix.git
  cd dcm2niix
  mkdir build 
  cd build
  cmake ..
  make
  
You will then want to add dcm2niix to your $PATH by typing the following into your command line:

::

  export PATH="$HOME/dcm2niix/build/bin/:$PATH"
  
You can check to see whether it is now in your $PATH, by typing the following, which should return the path to dcm2niix

::

  which dcm2niix
  

Regardless of which BIDS conversion software you choose, most run dcm2niix under the hood, so it's worthwhile to have. If you are working on your university/institution’s High Perfomance Computing supercomputer (HPC), you may have access to dcm2niix; however, you will want to check to ensure that you are using the most current version, as newer versions are more up to date with the BIDS specifications.

Disclaimer regarding dcm2niix: Running dcm2niix on a HPC can potentially take much longer (~6-10 min/session) than running on a local machine (~0.5-1.5 min/session), due to the poor disk I/O of HPCs. If this is the case for you, it may be worthwhile reaching out to your university/institution's HPC admin to see if there is an easy workaround.   
  
dcm2bids Installation
*********************

Dcm2bids is a package that takes the output from dcm2niix and organizes/renames files to meet the BIDS specification. To install, type the following on the command line, line by line:

::

  pip uninstall dcm2bids --user
  pip install dcm2bids --user
  
This should automatically add dcm2bids to your $PATH
  
pigz Installation (optional)
****************************

You can install pigz to reduce time during the file compression portion of dcm2niix, though it is not required for dcm2niix to run. The software may already be provided by your university/institution's HPC; however, if this is not the case then you will need to download it directly from their `website <https://zlib.net/pigz/>`__. Make sure the file is extracted/unzipped and in your Downloads folder. Then type the following onto the command line, line by line:


::

  mv ~/Downloads/pigz-2.4 $HOME
  cd $HOME/pigz-2.4
  make
  export PATH="$HOME/pigz-2.4/:$PATH"

Setting Up Your Configuration File
**********************************

This is where user input is paramount. Most BIDS converters require some type of configuration file or setup to tell the conversion software how to organize and name the output from dcm2niix. Before setting up the configuration file, a mock protocol is provided below, which details what a participant did during their scan session. This protocol contains both common data and modality types, as well as less common ones (the names of the individual acquisitions in the protocol are not important, merely descriptive). You do not need to type anything here.

::

  localizer
  anat-T1w
  fmap-SE-AP
  fmap-SE-PA
  func_task-bart_SBRef
  func_task-bart
  func_task-rest_SBRef
  func_task-rest
  localizer
  anat-T2w
  gre-field-mapping
  gre-field-mapping
  func_task-bart_SBRef
  func_task-bart
  func_task-rest_SBRef
  func_task-rest
  anat-FLAIR
  dwi-dir80-AP
  dwi-dir80-PA
  
A few notes about this protocol: There are two localizer acquisitions because the participant came out of the scanner halfway through the session for a short break. There are two different sets of field maps: *fmap-SE-AP* & *fmap-SE-PA* (spin echoes with opposite phase encoding directions), and *gre-field-mapping* & *gre-field-mapping* (magnitude and phase difference). Participants partook in two “tasks”: the Balloon Analogue Risk Task (bart), and resting-state (rest). Each functional run was preceded by a single-band reference (sbref). There were three separate anatomicals collected: T1w, T2w, and FLAIR. Lastly, two dwi scans were collected. Again, this mock protocol is meant to demonstrate the different kinds of acquisitions that can be collected during a scanning session.

To run dcm2bids, you will need to create a Javascript Object Notation (JSON) configuration file, which dcm2bids uses to determine which acquisitions in the protocol will be converted to BIDS. To create the configuration file, type the following onto the command line, line by line:

::

  cd $HOME
  touch BIDS_config.json


Based on the mock protocol for this tutorial, the configuration file should like this (you will need to copy and paste the contents below into the BIDS_config.json file:

::

    {
     "descriptions": [
        {
           "dataType": "anat",
           "modalityLabel": "T1w",
           "criteria": {
              "SidecarFilename": "002*"
           }
        },
        {
           "dataType": "fmap",
           "modalityLabel": "epi",
           "customLabels": "dir-AP",
           "IntendedFor": [
              4,
              6
           ],
           "criteria": {
              "SidecarFilename": "003*"
           }
        },
        {
           "dataType": "fmap",
           "modalityLabel": "epi",
           "customLabels": "dir-PA",
           "IntendedFor": [
              4,
              6
           ],
           "criteria": {
              "SidecarFilename": "004*"
           }
        },
        {
           "dataType": "func",
           "modalityLabel": "sbref",
           "customLabels": "task-bart_run-01",
           "criteria": {
              "SidecarFilename": "005*"
           },
           "sidecarChanges": {
              "TaskName": "bart"
           }
        },
        {
           "dataType": "func",
           "modalityLabel": "bold",
           "customLabels": "task-bart_run-01",
           "criteria": {
              "SidecarFilename": "006*"
           },
           "sidecarChanges": {
              "TaskName": "bart"
           }
        },
        {
           "dataType": "func",
           "modalityLabel": "sbref",
           "customLabels": "task-rest_run-01",
           "criteria": {
              "SidecarFilename": "007*"
           },
           "sidecarChanges": {
              "TaskName": "rest"
           }
        },
        {
           "dataType": "func",
           "modalityLabel": "bold",
           "customLabels": "task-rest_run-01",
           "criteria": {
              "SidecarFilename": "008*"
           },
           "sidecarChanges": {
              "TaskName": "rest"
           }
        },
        {
           "dataType": "anat",
           "modalityLabel": "T2w",
           "criteria": {
              "SidecarFilename": "010*"
           }
        },
        {
           "dataType": "fmap",
           "modalityLabel": "magnitude1",
           "IntendedFor": [
              11,
              13
           ],
           "criteria": {
              "SidecarFilename": "011*",
              "EchoTime": 0.00492
           }
        },
        {
           "dataType": "fmap",
           "modalityLabel": "phasediff",
           "IntendedFor": [
              11,
              13
           ],
           "criteria": {
              "SidecarFilename": "012*"
           },
           "sidecarChanges": {
              "EchoTime1": 0.00492,
              "EchoTime2": 0.00738
           }
        },
        {
           "dataType": "func",
           "modalityLabel": "sbref",
           "customLabels": "task-bart_run-02",
           "criteria": {
              "SidecarFilename": "013*"
           },
           "sidecarChanges": {
              "TaskName": "bart"
           }
        },
        {
           "dataType": "func",
           "modalityLabel": "bold",
           "customLabels": "task-bart_run-02",
           "criteria": {
              "SidecarFilename": "014*"
           },
           "sidecarChanges": {
              "TaskName": "bart"
           }
        },
        {
           "dataType": "func",
           "modalityLabel": "sbref",
           "customLabels": "task-rest_run-02",
           "criteria": {
              "SidecarFilename": "015*"
           },
           "sidecarChanges": {
              "TaskName": "rest"
           }
        },
        {
           "dataType": "func",
           "modalityLabel": "bold",
           "customLabels": "task-rest_run-02",
           "criteria": {
              "SidecarFilename": "016*"
           },
           "sidecarChanges": {
              "TaskName": "rest"
           }
        },
        {
           "dataType": "anat",
           "modalityLabel": "FLAIR",
           "criteria": {
              "SidecarFilename": "017*"
           }
        },
        {
           "dataType": "dwi",
           "modalityLabel": "dwi",
           "criteria": {
              "SidecarFilename": "018*"
           }
        },
        {
           "dataType": "dwi",
           "modalityLabel": "dwi",
           "criteria": {
              "SidecarFilename": "019*"
           }
        }
     ]
  }
  
To get the information into the file, you can type the following onto the command line:

::

  cd $HOME
  vim BIDS_config.json

Press the "i" key, and paste the contents into the file. To save and close the file, press the Escape button, and type the following: :wq
To ensure that the information was added and saved to the json file, you can type the following onto the command line:

::

  cat BIDS_config.json

  
Understanding dcm2bids’s configuration file
*******************************************

Let’s take a closer look at the configuration file we’ve just created (you can also refer to the `dcm2bids <https://cbedetti.github.io/Dcm2Bids/tutorial/>`__ tutorial and the `BIDS specifications <https://bids.neuroimaging.io/bids_spec.pdf>`__). Each acquisition has a ``dataType field``, which simply indicates the type of data. For example, anatomical data is indicated as ``anat``, functional as ``func``, field maps as ``fmap``, dwi as ``dwi``, etc. Next is the ``modalityLabel``, which more specifically stipulates the type of data. For example, if you have anatomical data, what is the modality of said data (e.g. T1w, T2w, FLAIR)? You will notice that some acquistion sections have a ``customLabels`` field; these are most commonly seen for field maps and functional acquisitions, and specify additionally required information regarding the acqusition. For example, spin echo field maps need to have their phase encoding direction listed (i.e. ``dir-AP``, ``dir-PA``, ``dir-LR``, ``dir-RL``, etc). Functional acqusitions (including corresponding sbref if they were also collected) need to have the task name and run number; these two pieces of information are separated via the underscore ``_`` symbol. Note that BIDS convention requires that resting-state acquisitions be given a task name (``task-rest``). Within the ``criteria`` section there can be multiple subsections. The most crucial is ``SidecarFilename``, which corresponds to the acquisition’s ``SeriesNumber`` (i.e. the chronological order in which the acquisition was collected in the protocol). This allows dcm2bids to determine which dicoms refer to which acquisition. There is also a subsection called ``sidecarChanges``, which is generally only needed for functional and magnitude/phasediff field map acquisitions. For functional acquisitions, the ``TaskName`` must be specified again, as this information gets injected into the corresponding run’s JSON file. For magnitude/phasediff, the ``magnitude1`` modality must contain its echo time, and the ``phasediff`` must contain the echo times of the two magnitudes. These pieces of information are also added to their corresponding JSON files.

The last field to discuss, and arguably the least straightforward, is the ``IntendedFor`` list, required for field map acquisitions. Simply put, the list contains the indices of the functional acquisitions that the field maps will perform susceptibility distortion correction (SDC) on. There are two important caveats in determing the functional indices: the first is that the indices must reflect a “revised” protocol that doesn’t include non-BIDS acqusitions (e.g. localizers), and second, since dcm2bids is performed in python the indices must reflect python indexing (where the first element is 0).

Let us refer back to our mock protocol, which contains two acquisitions that are not to be converted to bids - the localizers, which are absent in the configuration file. By removing the localizers from the protocol and listing the python-based indices, we can determine the functional indices needed for the ``IntendedFor`` list (don't type out section below).

::

  anat-T1w (0)
  fmap-SE-AP (1)
  fmap-SE-PA (2)
  func_task-bart_SBRef (3)
  func_task-bart (4)**
  func_task-rest_SBRef (5)
  func_task-rest (6)**
  anat-T2w (7)
  gre-field-mapping (8)
  gre-field-mapping (9)
  func_task-bart_SBRef (10)
  func_task-bart (11)**
  func_task-rest_SBRef (12)
  func_task-rest (13)**
  anat-FLAIR (14)
  dwi-dir80-AP (15)
  dwi-dir80-PA (16)

The starred acquistions show the functional data corresponding to the field maps. Functional indices 4 & 6 correspond to the first field map pair, and functional indices 11 & 13 correspond to the second field map pair. Note that single-band reference (sbref) acquisitions are not included in the ``IntendedFor`` field. If a single pair of field maps (regardless of type) were collected and the participant did not leave and re-enter the scanner during the session, then the ``IntendedFor`` list would contain the indices of all the functional acquisitions.


Running the dcm2bids command
****************************

Finally, we've created the configuration file; now the BIDS conversion can be performed. For this, we'll assume that the subject ID is 01 and that this is a single session study. Type the following on the command line

::

  dcm2bids_scaffold -o $HOME/BIDS_tutorial
  echo "study imaging data" > $HOME/BIDS_tutorial/README
  dcm2bids -d $HOME/BIDS_tutorial/BIDS_tutorial_data -p 01 -c $HOME/BIDS_config.json -o $HOME/BIDS_tutorial --forceDcm2niix
  
If however this were multi-session data (i.e. participant was scanned more than once), the dcm2bids command will look like this

::

  dcm2bids_scaffold -o $HOME/BIDS_tutorial
  echo "study imaging data" > $HOME/BIDS_tutorial/README
  dcm2bids -d $HOME/BIDS_tutorial/BIDS_tutorial_data -p 01 -s 01 -c $HOME/BIDS_config.json -o $HOME/BIDS_tutorial --forceDcm2niix

where the ``-s 01`` indicates that this is the first session. ``-s 02`` would indicate the second session, and so on.

Clean project folder
********************

Type the following onto the command line, line by line:

::

  mv $HOME/BIDS_tutorial/BIDS_tutorial_data $HOME/BIDS_tutorial/sourcedata
  rm -rf $HOME/BIDS_tutorial/tmp_dcm2bids
  
  
PyDeface (optional)
********
Although not required for BIDS compliance, it is highly recommended that users deface their anatomical image(s), such that facial features are removed from the image(s) to ensure a greater degree of anonymity for data sharing purposes. Since PyDeface is a python package, we recommend that users have one of the newer python 3 releases. To install PyDeface, type the following into your command line

::

  pip install pydeface --user
  export PATH="$HOME/.local/bin/:$PATH"
  
Check out the github page of `PyDeface <https://github.com/poldracklab/pydeface>`__ for additional information. 

In the mock protocol there are three separate anatomical acquisitions: T1w, T2w, and FLAIR. You would therefore need to run pydeface on each of these anatomicals. To deface the T1w you would type the following:

::

  pydeface $HOME/BIDS_tutorial/sub-01/anat/sub-01_T1w.nii.gz --outfile $HOME/BIDS_tutorial/sub-01/anat/sub-01_T1w.nii.gz --force
  
For multi-session data, the command would look like this:

::

  pydeface $HOME/BIDS_tutorial/sub-01/ses-01/anat/sub-01_ses-01_T1w.nii.gz --outfile $HOME/BIDS_tutorial/sub-01/ses-01/anat/sub-01_ses-01_T1w.nii.gz --force
  
PyDeface can sometimes takes upwards of 10 minutes to complete; however, the defaced image will have potentially identifying facial features removed without cutting into the brain. Defacing anatomical data will also in no way aversely affect the data. Tools such as MRIQC and fMRIPrep were developed using defaced anatomical data, and the removed features would be stripped away regardless during the skull-stripping step of pre-processing. 
  
Validating your BIDS data
*************************

Once the BIDS conversion is complete and the anatomicals defaced, you can use the `BIDS validator <https://bids-standard.github.io/bids-validator/>`__ to ensure that your data are BIDS-compliant. If there are any issues in how the data were converted, these will show up as either warnings (in yellow) or errors (in red). If there is an error, then it will need to be addressed, otherwise the data will likely not work on BIDS-apps such as MRIQC and/or fMRIPrep. Warnings are less pernicious, as you can potentially still run BIDS-apps on the data; however, at some point it will be worthwhile to address them.

NOTE: You will likely get an error when performing the BIDS validation due to a minor format issue in the dataset_description.json file. This is not important and can be ignored. 

Final Thoughts
**************

In the tutorial we created a dcm2bids configuration file based on our mock protocol. While it's tempting to assume that everyone's protocol will be the same, this is not always the case. As those who collect imaging data can attest, participants may need to randomly take a bathroom break, redo a functional acquisition due to excessive motion, etc. Thus, some participants' protocol will look different and therefore the configuration file will need to be adjusted to reflect this. Reducing user overhead is an ongoing process when it comes to BIDS conversion and continues to get better. While some work is required from the user's standpoint, you may come to realize that benefits of have a BIDS-compliant dataset outweigh the work put in.
