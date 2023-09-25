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
