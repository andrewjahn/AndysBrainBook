.. _Python_02_Types_Objects:

==========================================================
Python Tutorial #2: Types, Objects, and Control Statements
==========================================================

---------------

Object-Oriented Programming
***************************

Python is an object-oriented programming language, meaning that it primarily involves creating and manipulating objects. For example, we might create a circle as an object, and define certain characteristics: For example, its radius and circumference. We can then extend this concept to other objects, such as the images we use for fMRI analysis.

In the previous chapter, we learned the basics of Jupyter notebooks and began with assigning a value to a variable - that is, an object which can take any range of possible values. We now proceed to other *types* of variables, specifically built-in ones that come with Python by default. And the most common ones are integers, strings, and booleans.

Built-in Types
**************

Let's being with integers. These are any whole numbers (i.e., not fractions or numbers that contain a decimal). For example, you can assign integers to a variable using the method in the last chapter:

::

  NumClasses = 3
  StudentsPerClass = 20

Integers can be manipulated using any kind of arithmetic operation, such as multipication:

::

  TotalStudents = NumClasses * StudentsPerClass

And you can divide them as well:

::

  StudentsPerClass = TotalStudents / NumClasses

Which in this case returns a number with a decimal, indicating that it is a different kind of type called a *float*. 
