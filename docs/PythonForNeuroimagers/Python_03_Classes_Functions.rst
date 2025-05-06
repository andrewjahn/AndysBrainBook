.. _Python_03_Classes_Functions:

============================================================
Python Tutorial #3: Control Statements, Functions, & Classes
============================================================

---------------

Control Statements
******************

Now that you understand the basics of Python, we can use more advanced functions such as for-loops and conditional statements. These so-called *control statements* have been covered in the Unix and Matlab tutorial of this e-book, and for a more detailed summary of what they are, the reader is directed to those chapters.

To recapitulate, control statements direct the flow of variables and information within a Python script. You can think of them as analogous to traffic signs: They direct you where to go, when to speed up or slow down, and in general they control the flow of traffic. The two most common control statements you will come across are for-loops and conditional statements, which we now turn to.

For-loops
&&&&&&&&&

Imagine that you've just created a list with three elements:

::

  my_List = ["Hello", 21, "Python"]

And you want to enumerate the contents of the list, one element per line. You could type something like:

::

  my_List[0]
  my_List[1]
  my_List[2]

But that is tedious, and quickly becomes impractical for lists that contain dozens or hundreds of elements. Instead, you can do the same thing by using a *for-loop*, which will assign each element in the list to a variable, which can then be used in another command. For example, we could print each element in the list by typing:

::

  for x in my_List:
    print(x)

The first line is the **declaration** of the for-loop, in which each element of the list will be consecutively assigned to the variable ``x``. (You can choose whichever variable name you want.) The **body** of the for-loop, in this example, is ``print(x)``, which will return each element on a separate line:

::

  Hello
  21
  Python

The body of the for-loop can contain as much code as you want, manipulating the variable in whichever way you need.

You can also combine for-loops with the ``range`` command, which displays the index of the first and last element in the list, e.g.:

::

  num_elems = len(my_List)
  range(num_elems)

Which returns ``range(0, 3)``. (Remember that the second index, ``3``, is where the range stops; i.e., the index elements of the list are 0, 1, and 2.) To combine this with the for-loop, we can use something called string formatting to describe each element as it is printed:

::

  for i in range(num_elems):
    elem = my_List[i]
    print(f"Element {i}: {elem}")

You can vary the syntax by using what is called a **comprehension**, which simply inverts the order of the body and the declaration of the for-loop, enabling you to write a more compact version of the loop on a single line:

::

  my_Comp = [print(x) for x in my_List]

Which returns the same output as above.

You can see how for-loops can be very useful for fMRI data analysis, if all of the subjects are being analyzed the same way. We could, for example, create a preprocessing script with a placeholder variable for the subject name, and then use a for-loop to run one instance of the preprocessing script for each subject, using a single line of code. We will discuss this more in later chapters.

Conditionals
&&&&&&&&&&&&

You encounter many situations in everyday life in which your decision hinges on whether a certain condition is met or not. For example, if you have more than ten dollars, then you can get the extra guacamole on your burrito at Chipotle. Else, if you have between eight and ten dollars, just get a regular chicken burrito. Else, buy nothing and be hungry.

Although this seems like a silly example, it illustrates the concept of a **conditional statement**. Certain conditions are evaluated, and if they are met, then one decision is made; if the conditions are not met, then another decision is made. The burrito example above could be recast into a conditional statement in Python using the following code:

::

  money = 8.0

  if money > 10.0:
    print("Get the extra guac")
  elif money < 8.0:
    print("Go hungry")
  else:
    print("Get the regular chicken burrito")

Note how this will generate the appropriate response regardless of what value you assign to the variable ``money``.
