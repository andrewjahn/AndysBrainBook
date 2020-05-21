.. _Unix_05_ForLoops:

==============
Unix Tutorial #5: For-Loops
==============

.. note::

  Topics covered: variables, for loops, zero padding, semicolons
  
  Commands covered: for, seq



Overview
-------------

Neuroimaging analysis often involves running many commands, but with only a small alteration each time you run a new command. For example, you might have to analyze subject 1, then subject 2, subject 3, and so on. To save time, we will use something called a Loop - also known as a for-loop.

Let’s illustrate what this is with a simple example. Let’s say that you need to print the numbers 1, 2, and 3. You could do this by typing “echo 1” and hitting return; then echo 2, and echo 3. This gets the job done, but you can see that this would quickly become tedious if your goal was to print dozens or hundreds of numbers.

How can we make this easier? You probably noticed that each time we ran the command we changed the number after the echo command. A for-loop will automatically do this for you.

Here is an example of a for-loop to print the numbers 1, 2, and 3:
::

  for i in 1 2 3
  do echo $i
  done

Or, doing it in one line, with each section separated by semicolons:

::

  for i in 1 2 3; do echo $i; done

The for-loop has three sections, separated by semicolons. The first section is the Declaration: it begins by assigning the first item after “in” to the variable “i”; in this case, it would assign the value “1” to “i”. The numbers after “in” are called the “List”. The next section is the Body, which runs the commands written after “do,” replacing the variable with whichever value is currently assigned to the variable - for the first loop, this will be the number “1”. Since items remain in the list, the loop goes back to the declaration and assigns the next number in the list to the variable i; in this case, the number “2”. Then the body is run, and the process repeats until the end of the list is reached. The last section, called the End, contains only the word “done”, meaning to exit the loop after all of the items in the list have been run through the Body of the loop.

You can add more commands to the Body section, if they are separated with a semicolon. For example, we could change the loop to:

::

  for i in 1 2 3; do echo $i; echo “You just printed the number $i”; done
  
And this is what the output would look like:

::

  1
  You just printed the number 1
  2
  You just printed the number 2
  3
  You just printed the number 3

Looking ahead, eventually we’ll use for-loops to analyze twenty-six subjects, with directories named sub-01, sub-02, all the way to sub-26. We will use the loop to navigate into each directory and then run a script, but for now, imagine that we simply wanted to print the name of each directory. Something like this would work, but would also be tedious to write out:

::

  for i in sub-01 sub-02 … sub-26; do echo $i; done

This also quickly becomes impractical with large numbers. You can make this command more concise by using the seq command, which prints every number in a range that you specify; for example, seq 1 10 prints the numbers one through ten. We can include it in our for-loop using backticks, in which the command within the backticks is executed first and expanded:

::

  for i in `seq 1 26`; do echo “sub-$i”; done

In this case, the first run of the loop would assign 1 to i, print sub-1, and then go through the rest of the items in the list.

This gets us closer to our goal, but it still isn’t exactly what we want. Notice that the subject names each have two integers, such as 01, 02, 03, and so on, which ensures that each subject’s name is the same length; it also keeps them in order when they are listed with the ls command. This is called zero padding, and we can implement it in our for-loop with the -w option in seq, which looks like this:

::

  for i in `seq -w 1 26`; do echo “sub-$i”; done

This sets each number in this range to have a width of two integers; if it’s a number less than ten, for example, it is zero-padded with one zero to the left of the number. This will be important later on when we use these loops to automate analyses over all of our subjects.

-------

Exercises
**********

Today we covered the basics of for loops; later on, you’ll learn how to use them in more sophisticated contexts, such as automating the analysis of an entire dataset. But no matter how complicated the analysis, every for-loop is built on the fundamentals you learned today. Try these exercises to develop your understanding:

1. Type the following line of code: ``for i in `ls`; do echo $i; done``. Before you press Enter, think about what it will return. See if the output matches your prediction.

2. Write a for-loop to do the following for the numbers 1 through 3: Print the first number in the list, and then print the present working directory. Then, go up one directory. Repeat for all of the other numbers in the list.

3. Look up the syntax of a for-loop with tcsh, and use it to redo the examples above.


--------

Video
**********

Click `here <https://tinyurl.com/y6297v4e>`__ for an example of how to code for-loops.
