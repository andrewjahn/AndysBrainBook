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

    def __init__(self, name):
      self.name = name

The first line of code is simply the name of the class, capitalized by convention, followed by a ``def`` statement. We've seen this before with functions, and plays a similar role here too. What comes after, however, is something new: ``__init__``, which we will call a **method**. Methods are functions that are specific to a particular object, usually executed with dot notation. For example, in the previous tutorial, we used the ``randint`` method of the ``random`` object by typing ``random.randint(1,10)``. The ``__init__`` method in this case will be run anytime there is a new ``instance`` of the Class, which is like a copy.

.. note::

  Any method with double underscores on each side (such as ``__init__``) is technically a **magic method**, which are methods implicitly called during most operations in Python. The discussion of magic methods is outside the scope of this tutorial, but the curious reader is encouraged to read more about them from other webpages.

The arguments taken by the ``__init__`` method are ``self`` and ``name``. ``self`` is a convention used to refer to the class itself, indicating that all of the variables and attributes listed in the class should be used. The next argument, ``name``, will be supplied by the user when creating a new instance of the class.

At first glance, there appears to be redundancy in the code above, since the word ``name`` is used three times. This is the syntax needed to assign the name provided by the user to the instance of that class, e.g.:

::

  my_brain = Brain('andy')
  print(my_brain.name)

Which will return ``andy``.


Attributes
&&&&&&&&&&

Any variable or characteristic about a Python object is known as an **attribute**, which are divided into **class attributes** and **instance attributes**. Since we are working with a Brain class, let's give it a list of lobes, which any healthy brain should have. By declaring the ``lobes`` list above the ``__init__`` method, ``lobes`` functions as a class attribute which will be propagated to each new instance of the class. We can also expand upon our ``__init__`` method by providing a ``volume`` variable, with a default of 1400 cubic centimeters, which is an **instance variable** that can be changed when creating a new instance:

::

  class Brain:

    lobes = ['frontal', 'parietal', 'occipital', 'temporal']

    def __init__(self, name, volume=1400):
        self.name = name
        self.volume = volume


::

  my_brain = Brain('andy')
  print(my_brain.name + "'s brain has a volume of" + str(my_brain.volume))
  your_brain = Brain('susan', volume=1390)
  print(your_brain.name + "'s brain has a volume of" + str(your_brain.volume))

Which will print the corresponding name of each brain and volume of each brain. (Note that this uses the ``+`` symbol to concatenate variables and strings, and uses the ``str`` function to convert the brain volume from an integer to a string.) If, however, I have an accident which requires the removal of my temporal lobe, I can update the list of my lobes without affecting the same list that is propagated to every other instance of the class:

::

  my_brain.lobes = ['frontal', 'parietal', 'occipital']
  my_brain.lobes
  your_brain.lobes


Methods
&&&&&&&

Lastly, we can declare functions, which are called **methods** when they are bound to a particular class or module. The code appended to this class executes a conditional statement that determines whether the brain is thinking or not:

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


Summary
*******

This is a basic overview of classes, and there are many more details and nuances which you can discover on your own. Here we have reviewed the essentials, including instances, attributes, and methods, and you will come across these concepts as you continue to use Python. We now move another step up the hierarchy of Python to **modules**, which we turn to next.
