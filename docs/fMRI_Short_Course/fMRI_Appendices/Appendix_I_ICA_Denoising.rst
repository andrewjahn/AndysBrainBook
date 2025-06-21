.. _Appendix_I_ICA_Denoising:

==================================================================
Appendix I: Independent Components Analysis (ICA) with FSL and FIX
==================================================================

------------------

..  note::

    The methods in this tutorial are drawn from the FSL ICA website, which can be found `here <https://fsl.fmrib.ox.ac.uk/fslcourse/graduate/lectures/practicals/ica/>`__. I have also benefitted from the examples provided by Carline Nettekoven on `her blog <https://www.caroline-nettekoven.com/post/ica-cleaning/>`__.

Overview: What is Independent Components Analysis?
**************************************************

Independent Components Analysis, or ICA, is a method for decomposing a complex signal into simpler parts. For example, a convoluted time-course signal can be broken down into a series of sine waves which, if combined, would re-create the original signal.

Another example of ICA is using auditory software to tease apart different sources of noise. When recording an interview in the middle of a crowded room, for example, the software can identify different individual sources of noise from the original recording, such as other people's voices, background noise, and any noise induced by the recording equipment. These sources can then be filtered out of the original signal, resulting in a cleaner signal focusing on one source.

Conceptually, this is the same approach used when applying ICA to fMRI data. These datasets have both temporal and spatial components, and each of them can be extracted from the original data. Given that fMRI data also contain noise from a multitude of sources, such as physiological noise, movement artifacts, and scanner artifacts, ICA can be used to separate them from the BOLD signal we are interested in.

This tutorial will focus on analyzing a dataset with ICA, and then filtering out the noise components using FIX, another FSL tool. This can be particularly useful for cleaning data from high-moving populations, such as children and certain patient populations. For a good example of how FIX can be combined with nuisance regression, such as FD, see `this paper <https://www.sciencedirect.com/science/article/pii/S1878929322001219#fig0045>`__ by Jolinda Smith et al. (2022).


Analyzing the Data with ICA
***************************

To illustrate how to use ICA, we will be using `this dataset <https://openneuro.org/datasets/ds003871/versions/1.0.2>`__ which contains resting-state data from younger and older adults. For the training data, I will be focusing on subjects sub-1015 through sub-1024, if you want to download only those particular subjects.

If you would like to combine both preprocessing and ICA, you can navigate to the dataset main directory (containing all of the subjects) and type ``Melodic_gui``. This opens the MELODIC GUI, which contains the same tabs as a typical FEAT pipeline: Data, Pre-Stats, Stats, and Post-Stats. In this case, however, the Stats tab has options for different types of ICA.

We can analyze all of our subjects in one go, instead of running a separate instance of the GUI as we did with FEAT for each run of functional data. Gather all of your subjects' functional data by typing:

::

  ls $PWD/sub-*/func/*.nii.gz

And then paste these into the Data tab by changing the ``Number of inputs`` to 10, and clicking on ``Select 4D data`` and ``Paste``. Then click OK.

At this point you can specify an output directory. If you leave it blank, it will create a directory labeled with the subject's name and a ``.ica`` appended to it. We will leave it blank for now, and move to the Registration tab, keeping the defaults in the Pre-Stats tab.

Skull-strip your images using any method you choose. Assuming the skull-stripped brains have a ``_brain.nii.gz`` extension, you can obtain them from your command line by typing:

::

  ls $PWD/sub-*/anat/*_brain.nii.gz

And then paste these into the ``Select main structural images`` field. Click OK, and change the registration settings if you want - for example, ``Full search`` and ``12 DOF``.

In the Stats tab, leave the drop-down menu option as ``Single-session ICA``. If you want, you can leave ``Automatic dimensionality estimation`` checked, which will attempt to estimate an exhaustive number of components. In our case, let's uncheck the box, and change the number of ``Output components`` to 60. Then click ``Go``.

It will take some time for preprocessing and ICA estimation - maybe around an hour or so for all of the subjects. When it finishes, within each subject's ``.ica`` directory, there is another directory called ``filtered_func_data.ica``. This contains the ICA components that we can now label as either noise or signal.

Labeling the Components
***********************

To view these components, navigate to the ``.ica`` directory that contains the filtered_func_data.ica directory and type:

::

  fsleyes --scene melodic -ad filtered_func_data.ica

This will open a new fsleyes window with multiple axial slices and one of the components overlaid on it. The bottom left window displays a time-course for that component, and the bottom right window displays a power spectrum for different frequencies.

Label each component as either ``Unknown Noise`` or ``Signal``. If you are unsure, write ``Unknown``. Here are some features you can use to distinguish between Signal and Noise:

  Signal is usually:

  -Lower frequency
  -Has a smoother time-course
  -Has component loadings that are smooth, continuous, within the grey matter, and often bilateral

  Noise is usually:

  -Higher frequency
  -Has a more jagged, discontinuous time-course
  -Has component loadings within the white matter, CSF, or regions such as the eyeballs or Circle of Willis. Loadings that ring the edge of the brain are also indicative of motion artifacts.

Once you have labeled all of the components, save them into a file called "hand_labels_noise.txt", placing this file within the ``.ica`` directory that contains the filtered_func_data.ica folder (*not* the filtered_func_data.ica folder itself).


Training FIX on Your Data
*************************

Our next step is to a train a classifier on the components that we have labeled. First we will need to use the ``fix`` command to extract the features from our ICA datasets. For example, you could run this code from your experiment directory:

::

  for i in sub-10{15..24}; do cd $i/func; fix -f *.ica; cd ../..; done

Once this finishes, you can then train your model by typing:

::

  fix -t mymodel -l `ls -d $PWD/sub-10{15..24}/func/*.ica`

This will create a new model, mymodel.pyfix_model, which can then be used to clean any of the subjects in your dataset:

::

  fix sub-1015/func/sub-1015_task-rest_dir-AP_run-01_bold.ica mymodel.pyfix_model 20

In which the last parameter is the classification threshold. In general, thresholds of around 5-20 are moderate, while less than 5 is more liberal (i.e., allow more components, even if they are noise), and greater than 20 is more conservative.

Once this command runs, you will have a new file in your ``.ica`` folder called ``filtered_func_data_clean.nii.gz``. Compare this to the original filtered_func_data.nii.gz file, and see whether it cleans up different artifacts; it may also be useful to run a seed-based correlation analysis through the FSLeyes GUI in a robust network region to see how it compares before and after cleaning.
