.. _Unix_04_ShellsVariables:

=============
Unix Tutorial #4: Shells and Path Variables
=============

.. note::
  Topics covered: paths, variables, shells, FSL, installation, syntax, redirection
  Commands covered: set, setenv, export, tcsh, bash
  
  
Now that you’ve become more familiar with Unix commands, we can download an fMRI package and install it with Unix. If you haven’t already downloaded FSL, watch this video. When you’re done, come back to this tutorial. 

When you downloaded and installed FSL, you may have seen a few things you didn’t completely understand. For example, if you go to your home directory and type “cat .bashrc”, you’ll see this block of code (Highlight the FSL code block). To understand what’s going on here, you’ll need to understand shells, paths, and variables (fade each one in). First, let’s talk about shells. Think of the shell as an environment in which you can write Unix commands. We used a shell in the previous tutorials, but you may not have been aware of it. When you open the terminal, it uses a shell to interpret what you’re typing. Also, there are many different shells, and each one has a different syntax, or specific way that the words in your command need to be organized - just like in human languages. For example, there are two major shells - the Bourne shell, with a widely-used version called bash, or Bourne-again shell, and the C-shells, of which one popular variation is the t-shell, or tcsh (Fade these in). The commands we’ve used so far - cd, ls, pwd, and so on - are called built-in commands, and they can be used the same way in both shells. But there are important differences when you do a more advanced operation, such as setting a variable.


Video
---------

Click `here <https://www.youtube.com/watch?v=KAs94hs_aXY>`__ for a video walkthrough explaining what shells and path variables are.
