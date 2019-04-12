.. _Unix_04_ShellsVariables:

=============
Unix Tutorial #4: Shells and Path Variables
=============

.. note::
  This section is under construction; the video is complete, however.

.. note::
  Topics covered: paths, variables, shells, FSL, installation, syntax, redirection
  Commands covered: set, setenv, export, tcsh, bash
  
  
Now that you’ve become more familiar with Unix commands, we can download an fMRI package and install it with Unix. If you haven’t already downloaded FSL, watch `this video <https://youtu.be/E9FwDCYAto8?t=14>`__. After you’re done, come back to this tutorial. 

When you downloaded and installed FSL, you may have seen a few things you didn’t completely understand. For example, if you go to your home directory and type “cat .bashrc”, you’ll see this block of code. 

.. figure:: Bash_RC_Contents.png

To understand what this means, you’ll need to understand shells, paths, and variables. Let's begin with **shells**. Think of the shell as an environment in which you can type Unix commands. Or, think of it as an interpreter that translates what you type into the actual operations performed by the computer. We used a shell in the previous tutorials, but you may not have been aware of it. When you open the terminal, it uses a shell to interpret what you’re typing. Also, there are many different shells, and each one has a different **syntax**, or specific way that the words in your command need to be organized in order to be understood correctly - just like the syntax of human languages. 


There are two shells you will come across: the Bourne shell, with a widely-used version called **bash**, or Bourne-again shell;, and the C-shells, of which one popular variation is the t-shell, or **tcsh**. The commands we’ve used so far - cd, ls, pwd, and so on - are called built-in commands, and they can be used the same way in both shells. But there are important differences when you do a more advanced operation, such as setting a variable.

Setting a variable - also known as **variable assignment** - takes a string and assigns it a value. Variables are used as shorthand for a value, which can be either a number or a string. They are called variables because the value can vary, or be udpated as needed. 

For example, let’s assign the value 3 to the variable ``x``. If you are in the bash shell, which is the default on most computers, you can do this by typing ``x=3``. To check the value stored in the variable, type ``echo $x``. The dollar sign is a **reserved character** that has a special meaning and cannot be used as a variable. It indicates that what comes immediately after it - in this case, x - is a variable. The command returns 3, the value stored in the variable x.

Compare this with a different shell - the t-shell. Switch your terminal to the t-shell by typing ``tcsh`` and pressing enter. If we typed the same command as before, you’ll get an error that says “command not found.” That’s because the syntax for assigning a variable is different in the t-shell. To do the same variable assignment, we have to type ``set x=3``; then type ``echo $x`` to make sure it set the correct value. If you ever get lost and want to know which shell you are currently in, type ``echo $0``.


Now try the following exercises to review and consolidate what we’ve learned today. You’ll need to understand variables in order to use for loops, which we’ll talk about in the next video.

Exercises: 1) Change your default shell with chsh -s tcsh. What happens when you open a new Terminal and type echo $0? How would you change your default shell back to bash? 2) Look at these lines in your .bashrc file: “export FSLDIR=/usr/local/fsl”, and “export PATH=${FSLDIR}/bin:${PATH}. (For these lines of code, the curly braces don’t add anything; e.g., ${PATH} is the same thing as $PATH.) In your own words, how would you define what these lines do? 3) From your default Terminal (assuming you are in bash), launch a tcsh subshell. Then set an environmental variable, x=3. From your current shell, launch a bash subshell and type “echo $x”. Then type “exit”, and “exit” again to return to your original shell. Type echo $x. What is returned? Why? Using a Venn diagram, illustrate why this happens.



Video
---------

Click `here <https://www.youtube.com/watch?v=KAs94hs_aXY>`__ for a video walkthrough explaining what shells and path variables are.
