.. _Design_Optimization.rst

=============
Design Optimization
=============


Overview
-------------

fMRI experiments need to be designed slightly differently than typical behavioral experiments. If we take our Flanker experiment as an example, we can see the following differences between the behavioral experiment and the fMRI experiment:


Behavioral:

* Dependent measure of interest is reaction time
* Can have the same amount of time between trials
* Subject is in front of a computer, uses a keyboard to make responses

fMRI:

* Two dependent measures: reaction time *and* the :ref:`BOLD response <BOLD_Response>`
* Will need differing amounts of time between trials
* Subject is in the bore of the MRI, uses a button pad to make responses


Let's take a look at each of these differences in turn.


Dependent Measures
************

In both versions of the experiment, we have two conditions: Incongruent and Congruent trials. (See `here <fMRI_02_ExperimentalDesign>` for more details). We collect several trials, or instances of each condition. For example, if we have 5 trials in each condition, there would be 5 trials in which the subject saw an Incongruent stimulus, and 5 trials in which they saw a Congruent stimulus.

In the behavioral version, we would collect reaction times for each trial, and average them over each condition. Let's say that the reaction times (in milliseconds) were as follows:

Incongruent: 650, 670, 681, 591, 623 -> Average RT of 643ms
Congruent: 560, 571, 603, 580, 575 -> Average RT of 577.8ms

This is what we would expect from a typical subject: The average reaction time for Incongruent trials is greater than for Congruent trials. Or, to put it another way: Subjects are slower on average at making responses to the Incongruent trials as compared to the Congruent trials.
