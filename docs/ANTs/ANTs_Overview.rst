.. _ANTs_Overview:

===================================
Advanced Normalization Tools (ANTs)
===================================

-----------

ANTs is a software package for normalizing data to a template. Most of the templates provided on the ANTs website (maybe all of them?) are in MNI space. They can be downloaded from the `ANTs github page <https://github.com/stnava>`__. 

Below are some notes for how to use ANTs:

Downloading and Installing ANTs
*******************************

The most recent instructions for how to install ANTs on both Linux and Macintosh operating systems can be found `here <https://github.com/ANTsX/ANTs/wiki/Compiling-ANTs-on-Linux-and-Mac-OS>`__ on the ANTs github page. On my Macintosh operating machine, I do the following steps:

1. Make sure that `homebrew <https://brew.sh/>`__ is installed.

2. Install ``cmake`` by typing:

::

  brew install cmake
  
Which should either install the latest version of cmake, or update it to the latest version (as of this writing in early 2023, that would be version 3.26.3).

3. Using a Terminal, navigate to your home directory and download the installation script by typing:

::

  git clone https://github.com/cookpa/antsInstallExample.git
  
4. Run the script by typing bash installAnts.sh

This will take about 10 minutes to run. When it finishes, the binaries will be located in ``antsInstallExample/install/bin``. 

5. Organize the ANTs binaries by opening a Terminal and typing:

::

  cd ~
  mkdir ANTs
  cp -R antsInstallExample/install ANTs/
  cd ANTs
  mv install/* .
  rmdir install

You can then add these binaries to your PATH by typing:

::

  echo 'export ANTSPATH=/Users/ajahn/ANTs/bin/' >> ~/.bashrc
  echo 'export PATH=${ANTSPATH}:$PATH' >> ~/.bashrc
  
If you then type ``source ~/.bashrc``, or if you open a new Terminal, you should have access to all of the ANTs commands, no matter which directory you are in.

Skull-Stripping
***************

First, the anatomical image needs to be skull-stripped. You can do this with any skull-stripping algorithm, such as FSL's BET or AFNI's 3dSkullStrip, but for now we will only use ANTs commands:

::

  antsBrainExtraction.sh -d 3 -a sub-001_T1w_202.nii.gz -e brainWithSkullTemplate.nii.gz -m brainPrior.nii.gz -o anat_Stripped.nii
  
The option "-d 3" means that it is a three-dimensional image; "-a" indicates the anatomical image to be stripped; and "-e" is used to supply a template for skull-stripping. (which ones?) "-m" will generate a brain mask, and "-o" is the label for the output.


Or, you could use 3dSkullStrip:

::

  3dSkullStrip -input sub-001_T1w_202.nii.gz -prefix anat_ss.nii -push_to_edge
  
`optiBET <https://montilab.psych.ucla.edu/fmri-wiki/optibet/>`__ from the Monti lab is also an option:

::

  ./optiBET.sh sub-001_T1w_202.nii.gz
  
  
Inspect the resulting image to make sure the skull have been removed and the brain has been preserved.


