.. 02_Stats_HRF_History.rst

Chapter 2: History of the BOLD Signal
=========

Throughout the 1980's and early 1990's, neuroimaging researchers would measure contrast between brain tissues using methods such as positron emission tomography, or PET. This uses a radioactive glucose tracer that is absorbed by neurons when they are firing. By taking images of the brain under conditions of activity, such as seeing a flashing checkerboard or doing a cognitively demanding problem, researchers could see which regions were more active compared to other regions.

However, this method is invasive, and the idea of being injected with a radioactive tracer deterred many people from participating in such experiments. By the early 1990’s, an alternative imaging technique called magnetic resonance imaging (MRI) had become much faster and less expensive, and researchers were looking for a way to make it more widespread for clinical use. In 1990, Seiji Ogawa discovered that more deoxygenated blood leads to a decrease in the signal measured from a brain region. An increase in oxygenated blood, on the other hand, increases the signal - and this increase in oxygenated blood was shown to be caused by increased neural firing. This change in signal depending on oxygenation is known as blood oxygenation level dependent signal (or BOLD signal).

Shortly afterwards, in 1992, a researcher at Massachusetts General Hospital named Ken Kwong demonstrated that the BOLD signal could be used as an indirect measure of neural activity. His experiment consisted of alternately showing a flashing checkerboard and a black screen to the subject for one minute each. The BOLD signal was recorded during each condition, as shown in the following video:

.. figure:: Kwong_fMRI_Video.gif

This experiment was an important one, becoming the template for many functional neuroimaging experiments. Kwong had found a way to use blood inside the body as an endogenous tracer for imaging brain activity in healthy subjects, eliminating the need for injections or radiation. As a result fMRI experiments became more popular, and by the early 2000's fMRI had become the dominant neuroimaging method.


The BOLD Signal as an Indirect Measure of Neural Firing
********

Although the discoveries of Ogawa and Kwong were a boon for neuroimagers, there was a catch: This new method was an indirect measure of brain activity, a few steps removed from the actual neural firing. Whenever a stimulus is presented - such as a flash of light, or a sudden noise - that stimulus is transduced by the sensory organs into nerve impulses, which in turn stimulate neuronal firing in the brain. Neurons that fire require oxygen, and oxygen is delivered by the blood. That oxygenated blood in turn increases the signal from nearby hydrogen in the water in your body, which is what is measured in the scanner.

Nevertheless, this is the measure used to infer whether a given region of the brain is "active" or not. And to make those inferences, we will need to take a closer look at how the BOLD signal is genereated by a stimulus, and how we model these with mathematical functions.
