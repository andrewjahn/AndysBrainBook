.. _Appendix_G_ReverseInference:

=============================
Appendix G: Reverse Inference
=============================

------------------

Overview
********

Imagine that you see an fMRI activation map that looks like the following:

.. figure:: AppendixG_MotorActivity.png

Assuming that this is the only cluster that displayed any significant BOLD activity, and without knowing anything about the experiment that this data comes from, what would you assume the subject was doing at that time? Would you assume that the subject was listening to an audiobook, or that he was looking at a flashing checkerboard? What would be your best guess?

Most neuroimagers would think that the subject was making some kind of motion with his hand or fingers. Indeed, this image comes from a contrast of left button presses versus right button presses - a reliable way to elicit robust brain activity in the right motor cortex. This is not the only condition that could create such a contrast map, of course; it could be that the subject was merely imagining doing something with their left hand, or it could be that the subject was feeling some kind of stimulation in that hand, and due to imperfect normalization and a large smoothing kernel, the cluster may overlap with both the motor cortex and somatosensory cortices. In this case, though, the range of possibilites is relatively small, and the odds of guessing correctly are quite high. 

Now let's imagine that we see a BOLD contrast map that looks like this:

.. figure:: AppendixG_PainActivity.png

What you would assume now about what the subject was doing or experiencing? This becomes more difficult, since this is a pattern that could be elicited by many different scenarios: The coactivation of the dorsal anterior cingulate and the bilateral anterior insula are known to be active in response to cognitive control, pain, salience, surprise, and other common psychological phenomena that are studied using fMRI.

By assuming that one of these stimuli led to the brain activation map we are seeing, we are using a technique called **reverse inference** - that is, we are presented with a result or an end state, and we infer the cause from the data we are given. The flip side of this process is called **forward inference**, in which we know the cause or the stimulus that we gave to the participant - an electric shock, for example - and we predict where we will find BOLD activity in the brain.

In general, forward inference is the stronger reasoning of the two forms of inference, because we have control over the conditions that the subject experiences and, if the experiment is well-designed, we will account for any potential confounds, allowing us to rule out alternative explanations. Reverse inference, on the other hand, is more susceptible to biased interpretations if we are not careful.

For example, take a study by Nam et al., 2020, which correlated the grey matter volume of the amgydala with the likelihood of marching in a protest. Larger amygdala volume was associated with a lower likelihood of engaging in a political protest, and the reason for that is open to interpretation. If, however, you measured an individual's amygdala and found that it was exceptionally small, you would not have warrant for concluding that the person had participated in a poltiical protest; too many other potential variables are at play, and the explanatory power of just the amydala's grey matter, taken by itself, is quite small.
