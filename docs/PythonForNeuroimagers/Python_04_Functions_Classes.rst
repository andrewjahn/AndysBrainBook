.. _Python_04_Functions_Classes:

=========================================
Python Tutorial #4: Functions and Classes
=========================================

---------------

Functions
*********

So far in these tutorials, you have used blocks of code called **functions**, perhaps without knowing it. For example, when we typed ``len`` followed by a list, it returned the number of elements in that list. The command ``len`` is one of the **built-in functions** of Python, meaning that they are already available to you when you start any instance of Python.

Another such function is ``pow``, which raises one number to the power of another:

::

  pow(10,2)

This returns the value of 10^2, or 100. Note that a function has the following characteristics:

1. The name of the function, followed by parentheses;
2. One or more inputs, with each input separated by a comma.

These inputs are called **arguments**, and usually the function specifies how many arguments it can take; some functions require an exact number of arguments, while others only require one or two, and still others might not need any arguments at all. For example, we can learn more about the ``pow`` function and its arguments by typing ``help(pow)`` (note that ``help`` is another function):

::

  Help on built-in function pow in module builtins:

    pow(base, exp, mod=None)
    Equivalent to base**exp with 2 arguments or base**exp % mod with 3 arguments

    Some types, such as ints, are able to use a more efficient algorithm when
    invoked using the three argument form.


Note that this function requires at least two arguments, the base and the exponent, represented here by ``base`` and ``exp``. (You can also add a modulo, but the default is set to None, and we will ignore it for now.) If you enter two numbers, the function will raise the first number by the power of the second number, as we just saw. However, you can also explicitly specify which number belongs to which argument, by using the variable names specified in the output from the help file. For example:

::

  pow(2,10)

Would return 2^10, or 1024, while

::

  pow(exp=2, base=10)

Would return 10^2, or 100. In the latter case, when the arguments are assigned to the appropriate variable, the order of the arguments doesn't matter.

Building Your Own Function
&&&&&&&&&&&&&&&&&&&&&&&&&&

Now that you've seen how functions work, you probably want to create your own function. Let's begin by replicating the ``pow`` function by calling it something else. We begin by using the ``def`` keyword, followed by the name of our function:

::

  def my_power_function(base, exp):
    return base**exp

This is similar to the ``pow`` function above, with the body of the function containing a ``return`` keyword, followed by a mathematical formula that raises the base to the exponent (symbolized by ``**``). ``Return`` in this case means to return some value after the function is run, which can then be used by other code later on in the script. Once you have defined this function, try running it with some parameters, e.g., ``my_power_function(10,2)``.

When we used the ``help`` function earlier, you may have noticed that it output some text explaining how to use the function. We can include this explanatory text by using three sets of double quotes, which indicate that the text is not to be run as code, but simply to be read:

::

  def my_power_function(base, exp):
    """This function requires a base number and an exponent. The base will be raised to the power of the exponent
    Example: my_power_function(10,2)
    """
    return base**exp

Now when you type ``help(my_power_function)``, it will return the documentation as well.

Importing Other Functions
&&&&&&&&&&&&&&&&&&&&&&&&&

In addition to the built-in functions, Python also contains a library of other functions that can be accessed using the ``import`` command. The reason that all of these other functions aren't imported by default is that the user may not need all of them, and loading all of them when we open an instance of Python would take extra time.


