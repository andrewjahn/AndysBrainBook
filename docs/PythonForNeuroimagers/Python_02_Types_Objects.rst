.. _Python_02_Types_Objects:

===================================================
Python Tutorial #2: Types, Collections, and Objects
===================================================

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

Another important built-in type is *strings*. These are sequences of characters, and they are usually the same thing as words; in computer programming, however, they can take the form of compound words such as "movie_theater", which joins two words together with an underscore.

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

You can also count the number of instances of a given substring within a string, such as "r":

::

  Job.count("r")

Booleans
&&&&&&&&

The last basic data type is the Boolean. Named after the logician George Boole, these data types can only contain one of two values: "True" or "False". They are useful for determining whether a condition is met, for example, or whether a directory already exists before anything else is performed. You often see them used in control statements (discussed in more detail below) to check whether to exit the current processing in case an error is encountered or if the code is unnecessary.

One way we can check for the existence of something - a directory or a value, for example - is to use comparison operators such as greater than, less than, or equal to. For example, you could set a certain state to True or False:

::

  Studying_Python_Now = "True"

Or you can use comparison operators to test a claim:

::

  is_less_than_10 = len("Andy") < 10
  print(is_less_than_10)

To test whether two variables are equivalent, we use the double equal sign operator:

::

  my_Name = "Andy"
  your_Name = "Bill"
  same_Name = my_Name == your_Name
  print(same_Name)

You can also test for whether several tests are simultaneously true; if only one of them is false, then the output will return "False":

::

  ("hello" in my_Name) and (2 * 5 > 8) and (Studying_Python_Now == "True")

In this case the first comparison is False and the last two are True, so the output of this operation will be "False". 


Collections
***********

So far we have declared a few variables, and they are easy enough to remember and to work with. When you begin working with larger datasets, you will probably need to juggle dozens of variables and make sure they are updated appropriately. We can organize our variables using **Collections**, which are data structures that contain variables and other objects. The most common type of Collection is called a List, but there are others which we will explore below.

Lists
&&&&&

The **List** collection can contain any number of different types of objects: Strings, integers, and even other lists (a concept known as **Nested Lists**). For example, we could place the following variables within our list:

::

  my_List = ["Birthday", "Andy", "16", 20.5, 80]

Note that the list is enclosed in brackets, and that this list contains strings, a float, and an integer. The number "16" is enclosed in quotes, and thus will be treated as a string instead of as an integer. If you want to retrieve any of the items (also known as **elements**) within the list, you can do so with brackets, e.g.:

::

  my_List[1]

Which should return "Andy". Note that Python uses 0-based indexing, in which the first element in the list is indexed with 0, and all of the other elements are indexed consecutively. You can also use colons with the bracket notation in order to retrieve more than one element, e.g.:

::

  my_List[0:2]

Which will return the first two elements in the list. Note that the third element in the list (indexed by [2]) is not returned, since that side of the colon represents the stopping point of items to return. You can adjust the range as needed.

To create a nested list, let's create another list:

::

  another_List = ["penny", "cloud", 100]

And then join this together with our previous list:

::

  nested_List = [my_List, another_List]

We can now call upon any one of these lists, such as another_List, using index notation:

::

  nested_List[1]

And if we want to retrieve one of the elements within that list, we use another set of brackets:

::

  nested_List[1][0]

Dictionaries
&&&&&&&&&&&&

The **Dictionary** object is a data structure, similar to the kinds of structures found in Matlab. You can also think of a dictionary in Python as analogous to a book dictionary; each entry in the dictionary has a corresponding definition. Python dictionaries, however, are organized by pairings of **keys** and **values**, in which the keys have unique names (similar to each entry word in the book dictionary being different), but the values can be the same between keys.

As an example, let's say we create a dictionary representing the header information inside an fMRI dataset. This might contain parameters such as the TR, the voxel dimensions, and the number of volumes. We create this dictionary inside curly braces, with a colon separating each key/value pair:

::

  fMRI_Image = {
    "TR": 2.5,
    "voxel_dimensions": "3x3x3",
    "volumes": 150
}

If you just type the dictionary name, it returns all of the key/value pairings. You can also retrieve a single value by typing its key:

::

  fMRI_Image['TR']

And you can add to the dictionary by creating a new entry:

::

  fMRI_Image["orientation"] = "RPA"

Or overwrite one of the entries in the dictionary:

::

  fMRI_Image["TR"] = 2.0

Tuples
&&&&&&

The last type of collection is a *Tuple*, which is an immutable list; in other words, once you create the tuple, you can't change what's inside it. To create a tuple, use parentheses instead of brackets:

::

  my_Tuple = ("hello", 95, "A+")

Note that you can't overwrite any of these values, but you can convert a tuple to a list by using the ``list`` command:

::

  converted_Tuple = list(my_Tuple)
  converted_Tuple[2]


Summary
*******

We have covered some of the basic data types of Python, which are used in virtually any code that you will come across. In the following chapters, we will review more advanced aspects of Python, including control statements, functions, and classes, which give you greater flexibility for what your code can do.
