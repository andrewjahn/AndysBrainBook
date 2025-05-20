.. _Python_05_Classes:

===========================
Python Tutorial #5: Classes
===========================

---------------

Overview of Classes
*******************

For the casual Python user, or for the student who just wants to understand enough Python to know how to use the scripts that he downloads from Github, the preceding concepts are probably enough to get started. In order to write more advanced scripts and have greater flexibility for what you can do with Python, you will need to understand something called **Classes**.

As Python is an object-oriented programming language, virtually all of the code you work with in Python can be thought of as an object. And just as certain objects in everyday life can be placed in distinct categories, so can the objects in Python belong to specific classes. For example, think of an object such as a pencil. Some pencils are made of wood and graphite, while others are mechanical pencils made of plastic. Some pencils have sharper points, while others are more blunted. But they all share the common property of being a handheld tool used for writing.

Example of a Brain Class
************************

In the same way, Classes in Python can be thought of as templates for objects. These templates are convenient for creating copies which all have certain attributes, just like how parents have children that have similar characteristics as the parents.

Let's begin by creating a class called ``Brain``, and all we want to do is give it a name. The code to do that might look something like this:

::

  class Brain:

    def __init__(self, name)
      self.name = name

The first line of code is simply the name of the class, capitalized by convention, followed by a ``def`` statement. We've seen this before with functions, and plays a similar role here too. What comes after, however, is something new: ``__init__``, which we will call a ``method``. Methods are functions that are specific to a particular object, usually executed with dot notation. For example, in the previous tutorial, we used the ``randint`` method of the ``random`` object by typing ``random.randint(1,10)``. The ``__init__`` method in this case will be run anytime there is a new ``instance`` of the Class, which is like a copy.

.. note::

  Any method with double underscores on each side (such as ``__init__``) is technically a ``magic method``, which are methods implicitly called during most operations in Python. The discussion of magic methods is outside the scope of this tutorial, but the curious reader is encouraged to read more about them from other webpages.

The arguments taken by the ``__init__`` method are ``self`` and ``name``. ``self`` is a convention used to refer to the class itself, indicating that all of the variables and attributes listed in the class should be used. The next argument, ``name``, will be supplied by the user when creating a new instance of the class.

At first glance, there appears to be redunancy in the code above, since the word ``name`` is used three times. This is the syntax needed to assign the name provided by the user to the instance of that class, e.g.:

::

  my_brain = Brain('andy')
  print(my_brain.name)

Which will return ``andy``.


Attributes
&&&&&&&&&&

::

  class Brain:

    lobes = ['frontal', 'parietal', 'occipital', 'temporal']

    def __init__(self, name, volume=1400):
        self.name = name
        self.volume = volume

    def thinking(self, thinking='yes'):
        if thinking == 'yes':
          print("Congratulations, you are thinking!")
        elif thinking == 'no':
          print("You're not thinking right now; you are probably watching TV!")
        else
          print("I'm not sure what you're doing!")


::

  my_brain = Brain('andy')
  print(my_brain.name)
  your_brain = Brain('susan')
  print(your_brain.name)

Which will print the corresponding name of each brain. If, however, I have an accident which requires the removal of my temporal lobe, I can update the list of my lobes without affecting the same list that is propagated to every other instance of the class:

::

  my_brain.lobes = ['frontal', 'parietal', 'occipital']
  my_brain.lobes
