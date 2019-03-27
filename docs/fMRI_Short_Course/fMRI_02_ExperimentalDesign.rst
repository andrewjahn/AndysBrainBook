==============
fMRI Tutorial #2: Overview of The Flanker Task
==============

This dataset uses the Flanker task, which is designed to tap into a mental process known as cognitive control. For this course, we’re going to define cognitive control as the ability to ignore irrelevant stimuli in order to do the task correctly.

In the Flanker task, arrows point either to the left or the right, and the subject is instructed to press one of two buttons indicating the direction of the arrow in the middle. If it’s pointing to the left, the subject presses the “left” button; if it’s pointing to the right, the subject presses the “right” button. The middle arrow is flanked by other arrows which either point in the same direction as the middle arrow, or point in the opposite direction from the middle arrow.

.. figure:: Flanker_Example.png

	An example of the two conditions of the Flanker task. In the Incongruent condition, the central arrow (which the subject is focusing on) points in the opposite direction as the flanking arrows; in the Congruent condition, the central arrow points in the same direction as the flanking arrows. In this example the correct response in the Incongruent condition would be to push the "left" button, and the correct response in the Congruent condition would be to push the "right" button. To run through a version of the Flanker task yourself, click `here <http://cognitivefun.net/test/6>`__.

You can imagine that the task is easier if the central arrow points in the same direction as the flanking arrow, and more difficult if it points in the opposite direction. We’ll call the former condition the “Congruent” condition and the latter the “Incongruent” condition. Subjects are typically slower and more inaccurate in the Incongruent condition, and faster and more accurate in the Congruent condition. Since the difference in reaction times is robust and reliable, it follows that in our fMRI data we should see a noticeable difference in the :ref:`BOLD response <BOLD_Response>` as well.

.. figure:: Flanker_Design.png

	Illustration of the Flanker task for this study, adapted from Kelly et al. (2008). The subject is shown a fixation cross in order to focus on the center of the screen, and then either a Congruent or Incongruent Flanker trial is presented for 2000ms. During the trial the subject presses either the left or right button. A jittered interval follows which lasts anywhere from 8,000ms to 14,000ms. (Note that jittered intervals typically increment in seconds; in this case, the jitter for a given trial would be a random selection of one of the following: 8,000ms, 9,000ms, 10,000ms, 11,000ms, 12,000ms, 13,000ms, and 14,000ms) Another fixation cross is presented to begin the next trial.

Our goal is to estimate the size of the BOLD response to each condition, and then contrast (i.e., take the difference of) the two conditions to see whether they are significantly different from each other.

.. note::
	This brings up an important point about good practice for designing fMRI studies: If you can design a behavioral task that produces a strong and reliable effect, you will increase your odds of finding an effect in your imaging data. fMRI data is notoriously noisy - if you don’t see a behavioral effect in your study, you most likely will not find an effect in your imaging data either.
