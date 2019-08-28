.. _EP_03_Duration_Termination_Pre-Release:

===================================================
E-Prime Tutorial #3: Slides and Advanced Properties
===================================================

Overview
***********************

This tutorial will show you how to make a basic Stroop experiment. A new tool we will learn about is the Slide object - a versatile object that can contain several sub-objects, or objects within the object, such as multiple images and text boxes. The Slide object's properties are controlled by the property pages tab, while the sub-objects are controlled by the sub-object property pages tab.

Before we learn about Slides, we will revisit our TextDisplay objects and examine more advanced properties: Duration, Timing Mode, and Pre-release. To begin, click and drag a TextDisplay object to the experimental timeline, rename it to "Welcome", and type the following text in the textbox: "Welcome to the experiment! Press the SPACEBAR to continue." To make the text easier to read, change the background color to Black, the font size to 24, and the font color to White. (These can be found in the General tab - ForeColor corresponding to the font color, and BackColor to the background color - and the Font tab.) You should see the changes in the TextDisplay object occur as you make them.

At this point you could run the experiment by clicking the **Run** icon - a tiny purple man running through a door. When you are prompted to enter a Subject and Session number, use the defaults. You should then see the TextDisplay object briefly appear on the screeen and then disappear. This goes by too quickly to be of any use; how can we keep the text on the screen for a longer period of time, or only proceed when the subject makes a response?


Advanced Properties
***********************

Duration and Input Masks
^^^^^^^^^^^

By default, the TextDisplay object will be presented on the screen for 1000 milliseconds; in other words, one of its properties is a **duration** of 1000 milliseconds. This is probably too short for most purposes, and in any case you will want more flexbility for your own experiment. Within the TextDisplay object's properties, the ``Duration/Input`` tab allows you control these details about the timing of the object. The **Duration** field, for example, specifies how long, in milliseconds, the object will be presented on the screen. You can enter any number you want - or, you can set it to ``Infinite``, which will leave the object on the screen until a button is pressed or some kind of input is received.

This requires you to add an **Input Mask**, which is a device that records a response. If you click on the ``Add`` button underneath the Devices box, you will see two default options: the Keyboard and the Mouse. Since the participant usually responds by pushing one of several different keys, the Keyboard is the best input to use. Selecting the Keyboard as in input mask enables you to fill in the **Response Options** fields, such as Allowable and Correct responses.

In this case there is no Correct response, only an Allowable one: pressing the spacebar. To specify this we will have to use one of E-Prime's reserved keywords - in other words, a string of letters and symbols that indicates a specific key. To indicate the spacebar in this example, we will have to write the word "SPACE" in all capital letters, bookended by curly braces.

.. note::

  Other reserved keywords are "{ENTER}" and "{TAB}" (typed in the Allowable field without quotation marks). You may have noticed that the default in the Allowable field was "{ANY}". What do you think this means?
  
The **End Action**, set by default to "Terminate", indicates what to do once the subject makes a response. In this case, if he presses the spacebar, the TextObject will be removed from the display and the experiment will go on to the next object in the timeline. If there are no more objects in the timeline, E-Prime will exit the experiment. (The other options in the End Action menu - "None" and "Jump to" - will be covered later.) These options are summarized in the figure below.

.. figure:: 03_Duration_Inputs.png

  Selecting a Duration of Infinite (A) requires you to select an input device. Clicking on the Add button (B) allows you to choose from different inputs, such as a Keyboard or Mouse. Providing an Allowable response (C) enables the participant to make a response and to terminate the current object and move on to the next object (D).
  
  
Before moving on, drag another TextDisplay object onto the experimental timeline, placing it after the "Welcome" object. Rename the new object "Instructions", change the font to white and the background to black, and enter the following text:

::

  Press the 'f' key if the color of the word is blue.

  Press the 'j' key if the color of the word is red.

  Remember to respond to the color of the word, not the word itself.
  
As with the "Welcome" object, set the duration to ``Infinite``, selecting the Keyboard as an input and {SPACE} as an allowable response.


Timing Mode
^^^^^^^^^^^^^^^

Sometimes you want an object to be displayed until the subject makes a response; but other times, you want the object displayed for a specific amount of time - for example, fixation crosses that are displayed for a couple of seconds to signalize that the last trial is over, and a new trial is coming up. If the experiment is part of an fMRI scan, then accurate timing is especially important; you want the experiment to end at the same time as the scanner stops collecting images.

However, your computer is always running several processes simultaneously: Refreshing the monitor, checking for viruses, and keeping applications open in the background, to name a few. Each of these processes requires memory, and sometimes a process can temporarily require enough memory to put the other processes on hold. These processes are running even when an E-Prime experiment is being displayed on the screen, and these processes can introduce small delays into the experiment.

To illustrate how to use different timing modes, drag another TextDisplay object onto the timeline after the "Instructions" object. Rename it to "Fixation", and make it white font on a black background. Then open the Properties page and select the ``Duration/Input`` tab.

**Timing Mode** specifies how E-Prime will incorporate these delays. Selecting the "Event" option makes the object stay on the screen for as long as the Duration, no matter what delays there are before presenting the object. For example, if the Duration is 2000ms and there was a delay of 10ms when presenting the object, then 10ms will be tacked on to the duration of the object: 1000ms + 10ms = 1010ms. Select this option if you want every instance of that object to be the same duration; in this example, so that every Stroop trial will remain on the screen for exactly 1000ms.

Selecting the "Cumulative" option means that the object will last for the specified Duration minus any delays. For example, if the Duration is 1000ms and there was a delay of 10ms, then the duration of the object will be 1000ms-10ms = 990ms. Including the delay, the entire duration of the entire trial will be 1000ms: 990ms (object duration) + 10ms (delay) = 1000ms. This minimizes any cumulative timing errors, which may be important for experiments which need to end at an exact time for all the trials to be collected.

By expanding your tools to include Slide objects and more advanced options, you can create an entire Stroop experiment. In order to make this process more efficient, however, we next turn to Procedures and Lists to create larger-scale, more flexible experiments.

The Slide Object
*****************
