==============
fMRI Tutorial #2: Overview of The Flanker Task
==============

This dataset uses the Flanker task, designed to tap into a mental process known as cognitive control. For this course, we’re going to define cognitive control as the ability to ignore irrelevant stimuli in order to do the task correctly.

In the Flanker task, arrows point either to the left or the right, and the subject is instructed to press one of two buttons indicating the direction of the arrow in the middle. If it’s pointing to the left, the subject presses the “left” button; if it’s pointing to the right, the subject presses the “right” button. The middle arrow is flanked by other arrows which either point in the same direction as the middle arrow, or point in the opposite direction from the middle arrow. 

You can imagine that the task should be easier if the arrows all point in the same direction, and more difficult if they point in the opposite direction. We’ll call the former condition the “Congruent” condition and the latter the “Incongruent” condition. In behavioral studies, subjects are typically slower and more inaccurate in the Incongruent condition, and faster and more accurate in the Congruent condition. Since the difference in reaction times is robust and reliable, it follows that in our dataset we should see a noticeable difference in the :ref:`BOLD response <BOLD_Response>` as well.

Our goal is to estimate the size of the BOLD response to each condition, and then contrast (i.e., take the difference of) the two conditions to see whether they are significantly different from each other.

.. note::
	This brings up an important point about good practice for designing fMRI studies: If you can design a behavioral task that produces a strong and reliable effect, you will increase your odds of finding an effect in your imaging data. fMRI data is notoriously noisy - if you don’t see a behavioral effect in your study, you most likely will not find an effect in your imaging data either.
