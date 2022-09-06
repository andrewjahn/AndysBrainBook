.. _Matlab_04_ControlStructures:

======================================
Matlab Tutorial #4: Control Structures
======================================

----------

Overview
********

.. note::

  Topics covered: variables, for loops, if/else statements
  
  Commands covered: for, if/else, disp


Overview
--------

Neuroimaging analysis often involves running many commands, but with only a small alteration each time you run a new command. For example, you might have to analyze subject 1, then subject 2, subject 3, and so on. To save time, we will use something called a Loop - also known as a **for-loop**.

For-loops are examples of **control structures**: ways to specify the flow of control in a program. Let’s illustrate what this is with a simple example: Imagine that you need to print the numbers 1, 2, and 3. In Matlab you could do this by typing ``disp(1)`` and hitting return; then ``disp(2)``, and ``disp(3)``. This gets the job done, but you can see that this would quickly become tedious if your goal was to print dozens or hundreds of numbers.

How can we make this easier? You probably noticed that each time we ran the command we changed the number after the echo command. A for-loop will automatically do this for you.

Here is an example of a for-loop to print the numbers 1, 2, and 3:

::

    for i = [1 2 3]
    disp(i)
    end
    
Or, doing it in one line, with each section separated by semicolons:

::

  for i = [1 2 3]; disp(i); end

The for-loop has three sections, separated by semicolons. The first section is the **Declaration**: it begins by assigning the first item after the equals sign to the variable “i”; in this case, it would assign the value “1” to “i”. The numbers after “in” are called the **List**. The next section is the **Body**, which runs the commands written after the declaration, replacing the variable with whichever value is currently assigned to the variable - for the first loop, this will be the number “1”. Since items remain in the list, the loop goes back to the declaration and assigns the next number in the list to the variable i; in this case, the number “2”. Then the body is run, and the process repeats until the end of the list is reached. The last section, called the End, contains only the word “done”, meaning to exit the loop after all of the items in the list have been run through the Body of the loop.

You can add more commands to the Body section, if they are separated with a semicolon. For example, we could change the loop to:

::

  for i = [1 2 3]
  disp(i)
  i*i
  end

And the output would be:

::

  1
  1
  2
  4
  3
  9


Looking ahead, eventually we’ll use for-loops to analyze twenty-six subjects, with directories named sub-01, sub-02, all the way to sub-26. We will use the loop to navigate into each directory and then run a script, but for now, imagine that we simply wanted to print the name of each directory. Something like this would work, but would also be tedious to write out:

::

  for i = [sub-01 sub-02 … sub-26]

This also quickly becomes impractical with large numbers. You can make this command more concise by using the seq command, which prints every number in a range that you specify; for example, we can use a colon to specify the beginning and ending numbers, which will print those numbers and all the numbers in-between:

::

  for i = [1:26]; disp(['sub-' num2str(i)]); end

In this case, the first run of the loop would assign 1 to i, print sub-1, and then go through the rest of the items in the list.

This gets us closer to our goal, but it still isn’t exactly what we want. Notice that the subject names each have two integers, such as 01, 02, 03, and so on, which ensures that each subject’s name is the same length; it also keeps them in order when they are listed with the ls command. This is called **zero padding**, The text ``'%02d'`` is **string-formatting code** indicating that the current value being converted from a number to a string should be **zero-paddded** with as many zeros as needed until the number is two characters long. (Details about string formatting can be found `here <https://www.mathworks.com/help/matlab/matlab_prog/formatting-strings.html>`__.). We can implement it in our for-loop, which looks like this:

::

  for i = [1:26]; disp(['sub-' num2str(i, '%02d')]); end

This sets each number in this range to have a width of two integers; if it’s a number less than ten, for example, it is zero-padded with one zero to the left of the number. This will be important later on when we use these loops to automate analyses over all of our subjects.

-------

Exercises
*********

Today we covered the basics of for loops; later on, you’ll learn how to use them in more sophisticated contexts, such as automating the analysis of an entire dataset. But no matter how complicated the analysis, every for-loop is built on the fundamentals you learned today. Try these exercises to develop your understanding:

1. Type the following line of code: ``for i = ls; disp(i); end``. Before you press Enter, think about what it will return. See if the output matches your prediction.

2. Write a for-loop to do the following for the numbers 1 through 3: Print the first number in the list, and then print the present working directory. Then, go up one directory. Repeat for all of the other numbers in the list.

Next Steps
**********

Now that you've completed a short tutorial on the fundamentals of Matlab, you are ready to begin applying these concepts to SPM. I recommend going through the SPM12 tutorial on this website, and in particular reading the chapter on :ref:`scripting in Matlab <SPM_06_Scripting>`. 
