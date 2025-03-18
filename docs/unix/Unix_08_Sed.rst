.. _Unix_08_Sed:

Unix Tutorial #8: The Sed Command
================

.. note::

  Topics covered: Stream editing, swapping
  
  Commands covered: sed
  

Overview
**********

The commands you have learned so far will allow you to create flexible scripts that can be adapted to many different scenarios. There is one more command you will learn to round out your toolkit of Unix commands: The ``sed`` command.

Sed is an abbreviation for "stream editor", in that the input to sed is a stream of text - the same concept as the input and output streams that were discussed in a :ref:`previous chapter <Unix_03_ReadingTextFiles>`. Our goal is to take a stream of input text and replace one string with another. Sed's advantage over doing a similar procedure with for-loops is that sed can edit a file and only change certain words while overwriting the file and leaving the rest of the text intact.

Example with Sed
**********

To see how sed works, create a text file called ``Hello.sh`` that contains the following line:

::

  #!/bin/bash
  
  echo "Hello, my name is Andy. Here is the name Andy again."
  

This is simply a text file that runs one line of code. If you wanted to swap the name Andy with Bill, you would type the following:

::

  sed "s|Andy|Bill|g" Hello.sh
  
Notice that the sed command is divided into three sections:

1. Declaring the sed command;
2. A pattern to match and replace with another pattern, enclosed in quotes;
3. The file to be read into sed (in this case, Hello.sh)

Let's focus on the pattern section. I prefer to enclose this section in double quotes, so that if I include a variable in the pattern, it will be expanded before the sed command is run. The first part of this section is an "s", which means to "swap" the following pair of strings. The first of the pair is what is being searched for in the text file, and the second of the pair is what it will be replaced with. The "g" stands for "global", which means to replace every instance of the first word with the second word.

If you run this command, you should see the following output:

::

  #!/bin/bash
  
  echo "Hello, my name is Bill. Here is the name Bill again."
  
If you wanted to redirect this output into a new text file, you would use this code:

::

  sed "s|Andy|Bill|g" Hello.sh > Hello_Bill.sh
  
As always, you can call the output file whatever you like.

Editing Files In Place
**********************

If you want to edit the file and overwrite it instead of redirecting the output into a new file, you can use the -i and -e options:

::

  sed -i'' -e "s|Andy|Bill|g" Hello.sh

The -i option stands for "in-place", and signifies that the text file should be overwritten after the words have been swapped. The `''` after `-i` is a null argument that would otherwise be appended to the output of the changed file.

If you would like to make a copy of the file that contains the change, then you could write something like:

::

  sed -i- -e "s|Andy|Bill|g" Hello.sh

Which will create a file called "Hello.sh-".

I am indebted to Charles Antonelli of the University of Michigan's Advanced Research Computing department for these examples.


Using sed with for-loops
************************

As with other commands, sed can be combined with for-loops and conditional statements to write more sophisticated code. For example, let's say that we want to create several copies of a template file, and only change one word of it over a list of names. Let's start by creating a file called ``Names.sh`` which contains the following:

::

  #!/bin/bash
  
  echo "Hi, my name is CHANGENAME."
  

Here, CHANGENAME is a placeholder; I've typed it in all capital letters to make it stand out, which is especially useful in larger text files. Now we can use a for-loop to create several copies of this file, replacing CHANGENAME with whichever name is currently in the loop:

::

  for name in Andy John Bill; do
    sed -i -e "s|CHANGENAME|${name}|g" Names.sh > ${name}_Names.sh
  done
  
Before you type this code and run it, think about what will happen. Visualize how the items in the list will replace the variable ${name}, and how this will be swapped with CHANGENAME in the Names.sh file.

Now run the code. Do you get the output you expected? Why or why not?


----------

Exercises
*********

1. The sed command can use any character for a file separator; for example, try this code with the Hello.sh script:

::

  sed "s/name/last name/g" Hello.sh
  
Now replace the forward slash with some other character. Which separators (also known as delimiters) seem better than others? Why? When would a forward slash separator be problematic?


2. You can delete a line in sed by changing the last ``g`` to a ``d``. When using sed to delete a line, you must 1) remove the initial ``s``, and 2) only use forward slashes as delimiters. For example, if you wanted to delete a line containing the string "name", you would type:

::

  sed "/name/d" Hello.sh

Knowing this, download the `Make FSL Timings <https://github.com/andrewjahn/FSL_Scripts/blob/master/make_FSL_Timings.sh>`__ script, and use sed to delete any lines that contain the string ``run-1``. Compare the output to what was in the script before you ran sed.

---------

Video
***********

Click `here <https://www.youtube.com/watch?v=TkVhtWgim8M>`__ for an screencast overview of the sed command. 



