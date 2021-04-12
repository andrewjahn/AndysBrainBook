.. _Slicer_02_Motive:

==========================
Slicer Tutorial #2: Motive
==========================

----------

Overview
********

Motive is a software package that is used with the OptiTrack tracking system. For example, in this walkthrough I am using an OptiTrack Trio system, which contains three cameras. This setup is designed to provided high-resolution imaging on an object from different angles, in order to use it with a tracking object such as a **stylus**.

.. figure:: 02_Trio.png

  The OptiTrack Trio system.

The OptiTrack cameras are sensitive to high-reflective surfaces, which will show up in high contrast to the background. Small, reflective-coated balls called **markers** are placed on the objects you want to image, such as the stylus or the reference object. Since the relative distance between the markers of an object should be constant, OptiTrack will be able to track them no matter at what angle the object is presented.

.. figure:: 02_Stylus.png

Creating the XML File
*********************

Previous versions of Motive (1.x) used **projects** to organize the data that was being viewed at a given time. For more recent versions of Motive (2.x), the data is saved as a **profile** in .xml format.

