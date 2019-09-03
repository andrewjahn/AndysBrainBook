.. _07_SlideStates:

=============================================
E-Prime Tutorial #7: Slide States
=============================================


Overview
***********************

In the previous tutorial, you were introduced to the concept of **Slide States**. That is, one slide can contain many states, or stimuli that can be displayed depending on another condition being met - such as whether the response on a previous slide is correct or incorrect.

Put another way, Slide States are different instances of the same Slide object. For example, you may want to present two types of conditions, one where the subject responds to the color of a word, and another condition where the subject responds to the color of a car. Instead of creating two separate Slide objects, you can put both conditions into a single Slide, making the experiment more compact and more flexible.

You can create Slide States by clicking on the Add Slide State button in the Slide object toolbar. The SlideState that is presented depends on the ActiveState field, which is found in the property pages of the Slide. The SlideState names can be changed through the sub-object properties pages, and then the ActiveState can point to any one of the SlideStates.

.. figure:: 07_SlideStates.png

Instead of pointing to just one SlideState, we can have the ActiveState depend on which condition is chosen from a List object. Create another attribute in the Slide object and enter the names of the SlideStates. In our example, we created a new attribute called "StateName", and have the ActiveState field refer to this attribute when presenting a trial. In our example, if the StateName attribute is "Car", then the Car SlideState is activated; if the StateName attribute is "Stroop", then the Stroop SlideState is activated.

.. figure:: 07_StateName.png


Once you've mastered these basics of E-Prime and object-oriented programming, you have the tools to create even more sophisticated experiments. The next tutorial will expand the structure of our experiment by including a separate practice session in addition to the experimental trials. To do this, we will use Inline objects, which give you even greater power and flexibility in building your experiments.


---------------

Video
********

For a video review of Slide States, click `here <https://www.youtube.com/watch?v=q_h6qYjK3d0&list=PLIQIswOrUH68zDYePgAy9_6pdErSbsegM&index=7>`__.
