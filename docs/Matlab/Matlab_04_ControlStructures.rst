.. _Matlab_04_ControlStructures:

======================================
Matlab Tutorial #4: Control Structures
======================================

----------


.. note::

  Topics covered: variables, for loops, if/else statements
  
  Commands covered: for, if/else, disp


For-Loops
*********

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

This gets us closer to our goal, but it still isn’t exactly what we want. Notice that the subject names each have two integers, such as 01, 02, 03, and so on, which ensures that each subject’s name is the same length; it also keeps them in order when they are listed with the ls command. This is called **zero padding**, The text ``'%02d'`` is **string-formatting code** indicating that the current value being converted from a number to a string should be **zero-padded** with as many zeros as needed until the number is two characters long. (Details about string formatting can be found `here <https://www.mathworks.com/help/matlab/matlab_prog/formatting-strings.html>`__.). We can implement it in our for-loop, which looks like this:

::

  for i = [1:26]; disp(['sub-' num2str(i, '%02d')]); end

This sets each number in this range to have a width of two integers; if it’s a number less than ten, for example, it is zero-padded with one zero to the left of the number. This will be important later on when we use these loops to automate analyses over all of our subjects.


Conditional Statements
**********************

Another important control structure is the **conditional statement**, also known as an **if/else statement**. The code within a statement is run only if a certain condition is satisfied. For example, you might decide that you will go out to eat if you have $20 or more in your pocket; else, you will eat at home. If on the other hand you have very little money, you will feel motivated to make more. We can represent this as a conditional statement in Matlab by typing:

::
  
  money = 20;
  if money >= 20
    disp('Eat at restaurant')
  elseif money < 5
    disp('Make more money')
  else
    disp('Eat at home')
  end


.. note::

  If you type the above code line by line, it will terminate after the first line of code in the conditional statement is entered. You can copy and paste the above code, or you can type it all in one line, using semicolons to separate each line of code:
  
  ::
  
    money = 20; if money >= 20 disp('Eat at restaurant'); elseif money < 5 disp('Make more money'); else disp('Eat at home'); end
  
  Or, you could type the code into a new script and run it from there.
  
The structure of the conditional statement is straightforward: We first declare a variable if we don't have one already, since the conditional statement will need one, and then we begin the conditional with the word ``if``. A **comparison operator** is used to compare the value of the variable to some value set by the user; and if the condition is true, the code below it is executed. If the condition is false, there is another statement begun with the word ``elseif``, and that condition is checked. If neither of the first two conditions are true, then the code after the word ``else`` is run. Lastly, the keyword ``end`` is used to close the statement.

If we change the value of the variable money to be 10 (i.e., ``money = 10``) and run the conditional statement again, notice that you now get a different result, since the second conditional statement was triggered.

We can make our conditional statements more sophisticated by adding other operators as well. Let's say that if you have less than five dollars, but you have a rich friend, you will eat at a nice restaurant. We can symbolize this with two ampersands (``&&``), which will only execute the code if both conditions are true on either side of the ampersand:

::
  
  money = 3; richFriend = 1;
  if money >= 20
    disp('Eat at restaurant')
  elseif money < 5 && richFriend == 1
    disp('Eat at fancy restaurant')
  else
    disp('Eat at home')
  end
  
In this case we define a new variable, ``richFriend``, as having a value of ``1`` - in other words, that the rich friend exists. We expand the second conditional statement by adding ``&& richFriend == 1``, which uses two equals signs to test whether the value contained in the variable ``richFriend`` is equal to ``1``. If both the statements ``money < 5`` and ``richFriend == 1`` are true, then the code below that condition is executed.

Conditional statements can also be used to check whether something is true or false. In Matlab terms, a binary variable that can be either true or false is called a **boolean**. For example, we may want to check whether a file has been unzipped (i.e., decompressed) or not, and if it hasn't, we want to unzip it. Click `here <https://github.com/andrewjahn/PSYCH808>`__, and then click on the file ``anat.nii.gz``. Click on the ``Download`` button to download it to your computer. Then, type the following code in a Matlab terminal to move it to your current directory:

::
  
  movefile('~/Downloads/anat.nii.gz', '.')
  
Next, we will want to check whether it has been unzipped by using the ``isfile`` command; that is, checking whether a certain file exists or not. If it does exist, the ``isfile`` command will return a value of ``1``; else, it will return a value of ``0``. In this example if we wanted to check whether the unzipped file existed, and if not, to unzip it, we could type:

::

  if isfile('anat.nii') == 0
    disp('Anatomical image has not been unzipped; unzipping now')
    gunzip('anat.nii.gz')
  elseif isfile('anat.nii') == 1
    disp('Anatomical image is already unzipped; doing nothing')
  end

Later on in the SPM tutorials, we will be using conditional statements later on to create a script that automatically analyzes our subjects.

-------

Exercises
*********

Today we covered the basics of for loops; later on, you’ll learn how to use them in more sophisticated contexts, such as automating the analysis of an entire dataset. But no matter how complicated the analysis, every for-loop is built on the fundamentals you learned today. Try these exercises to develop your understanding:

1. Type the following line of code: ``for i = [1:3:10]; disp(['sub-' i]); end``. Before you press Enter, think about what it will return. See if the output matches your prediction. What needs to be added to the code in order to display the appropriate subject numbers (e.g., sub-01, sub-04, etc.)? Show the code you used, and the output.

.. for i = [1:3:10]; disp(['sub-' num2str(i,'%02d')]); end

2. To begin this exercise, first type ``cd ~`` from the terminal to make sure you are in your home directory. Then, write a for-loop to do the following for the numbers 1 through 3: Print the first number in the list, and then print the present working directory. (Printing the working directory can be done with the code ``disp(pwd)``.) Then, go up one directory, print the present working directory, and go back into the home directory. Make sure this happens for each number in the list. Show the code and the output.

.. for i = [1:3]; disp(num2str(i)); cd ..; disp(pwd); cd ~; disp(pwd); end

3. We can use conditional statements to determine whether a certain preprocessing step should be run. For example, we might decide to use slice-timing correction if the repetition time for a volume is 2 seconds or greater (slice-timing correction, and preprocessing more generally, will be discussed in more depth in the SPM module). From the previous chapter, make sure you have loaded the SPM file by typing ``load SPM_SingleSubj.mat``. The repetition time can be found in the field ``SPM.xY.RT``. Create a conditional statement that will display the text ``Perform slice-timing correction`` if the repetition time is 2 seconds or greater, and display the text ``Do not perform slice-timing correction`` if the repetition time is less than 2 seconds. Show the code you used.

.. Something like this: if SPM.xY.RT < 2 disp('Do not perform slice-timing correction'); else disp('Perform slice-timing correction'); end. Other ways of coding this are acceptable, such as if SPM.xY.RT >=2 disp('Perform slice-timing correction'); else disp('Do not perform slice-timing correction'); end.

Next Steps
**********

Now that you've completed a short tutorial on the fundamentals of Matlab, you are ready to begin applying these concepts to SPM. I recommend going through the SPM12 tutorial on this website, and in particular reading the chapter on :ref:`scripting in Matlab <SPM_06_Scripting>`. 
