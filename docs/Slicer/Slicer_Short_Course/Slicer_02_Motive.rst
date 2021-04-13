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

When you first open Motive, you will see the following layout:

.. figure:: 02_Motive_Layout.png

The top window pane is the **Perspective View**, a combination of all of the three cameras on a viewing plane. The three windows highlighted in orange are the views in each individual camera; the default, **Precision View**, is sensitized to the reflective markers, which show up as bright dots against a black background. For a better picture of the entire scene, you can right-click in any one of the viewing windows and select **Grayscale**, which will display a grayscale recording from that particular camera's feed:

.. figure:: 02_Motive_Grayscale.png

