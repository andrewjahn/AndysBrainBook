.. _EP_04_Lists_Procedures_Attributes:

======================================================
E-Prime Tutorial #4: Lists, Procedures, and Attributes
======================================================


Expanding the Experiment
************************

So far we have created the bare minimum of an experiment: We have an introductory screen; the participant sees a screen with instructions about how to perform the task; a fixation screen signalizes that a trial is about to begin; and a Slide object presents the word "blue" in red text, to which the participants need to override their impulse to respond to the word, and respond to the color instead. Afterwards a Text Object ties up the experiment and bids the subject farewell.

This is the skeleton of any experiment, and can be used as a template for virtually any study that you have in mind. The only problem lies in how to extend this template to include as many trials as we want. If we required one hundred Stroop trials in our experiment, it would be tedious to continue manually inserting the same sequence of Fixation -> Stroop.

The List Object
**************

Fortunately, E-Prime has a feature that allows us to loop through several instances of the same or similar trials: The **List Object**. The List Object will repeat all of the objects that come after it as many times as we like; we can also indicate how many trials of a particular condition we would like to display in the experiment, and which properties we would like to change on a given trial - such as the color of the text.

To illustrate this, click and drag a List Object from the Toolbox sidebar onto the experimental timeline and place it between the Instructions and Fixation Text Objects, and then rename it to "StroopList". If you double-click on it, you will see a window that contains both rows and columns. The rows we will refer to as **Levels**, and the columns will be called **Attributes**. Levels correspond to trials, and the Attributes are variables that we can specify for each trial. Attributes can be used in any of the Objects of your experiment, and all Attributes will be saved into the output files at the end of the experiment.
