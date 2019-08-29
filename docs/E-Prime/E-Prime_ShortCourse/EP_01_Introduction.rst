.. _EP_01_Introduction:

=================
E-Prime Tutorial #1: Introduction to E-Prime
=================


E-Prime is a stimulus presentation program that can display text, images, and videos; it is able to collect participant responses and calculate accuracy and response time.

There are several other stimulus presentation programs, such as `PsychoPy <https://www.psychopy.org/>`__ and `Presentation <https://www.neurobs.com/menu_presentation/menu_features/features_overview>`__. E-Prime is commercial software and costs around $1,000 for a single license; PsychoPy is free and has many of the same features, but may be more difficult to learn for students new to programming.

In any case, this tutorial will focus on E-Prime, since it is one of the most popular stimulus presentation packages. E-Prime has graphical user interface that can be used to quickly create a simple experiment, and also allows the user to insert **E-basic** code to perform more complex operations. This code will be reviewed in a later tutorial on **Inline objects**.

.. figure:: 01_EPrime_Layout.png

  The layout of a typical E-Prime experiment. The sidebars on the left contain a list of objects that can be placed on the experiment timeline (the "Toolbox" sidebar); a list of properties for the currently selected object, such as the name, duration, and color ("Properties"); a representation of the experiment from start to finish ("Structure"); and a space for the currently selected object.
  
The backbone of an E-Prime experiment is the **procedure** object; each new E-Prime experiment starts with a procedure objects called **SessionProc**. If you double-click on the object to see its contents, you will see a window that contains a horizontal line - the **Procedural Timeline**. It depicts the timeline of the experiment going from left to right. As we construct our experiment with **objects**, the timeline will present each object in sequence as it appears on the timeline. How these objects are constructed and how we use them to build an experiment, we will turn to in the next chapter.
  
  
Video
**********

For a video overview of what E-Prime is and what it looks like, click `here <https://www.youtube.com/watch?v=t3hZHveUVE8&list=PLIQIswOrUH68zDYePgAy9_6pdErSbsegM>`__.
