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

Integers
&&&&&&&&

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

Which in this case returns a number with a decimal, indicating that it is a different kind of type called a *float*. You can also use Python's ``type()`` command to determine what type a variable is, e.g.:

::

  type(NumClasses)
  type(StudentsPerClass)

Notice how the type changes depending on whether the variable contains a decimal point or not; if it does, it will be float, and if it does not, it will be an integer.

Strings
&&&&&&&

Another important built-in type is *strings*. These are sequences of characters, and they are usually the same thing as words; in computer programming, however, they can take the form of words such as "movie_theater", which joins two words together with an underscore.

You can assign a string to a variable the same way we did with integers:

::

  Job = "Neuroimager"

You can use the ``len()`` command to return how many characters are in the string:

::

  len(Job)

And you can change the case of the string by using the ``.lower()``, ``.upper()``, and ``.capitalize()`` commands (Note that these are *methods*, which are appended to the end of the variable):

::

  Job.upper()
  Job.lower()
  Job.capitalize()

