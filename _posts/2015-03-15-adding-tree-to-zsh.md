---
layout: post
title:  Adding the Tree Command to Zsh
---
## Background
For those who are unaware, the tree function is a useful shell command that displays
the file structure of a given directory in a visual manner. For example, if I am
in a directory titled my_blog and within that directory have three folders html, css
and js, tree would show something like this:  

      |---html  
      |---css  
      |---js  


Obviously in this example, tree isn't all that useful. But when dealing with large,
complex directories, the tree command is very helpful.

##Adding Tree
For the sake of simplicity, I chose to use an alias for the tree command rather than creating
a new directory and altering my function search path to read that directory. I searched the
web for a bit and found [a solution](http://www.kingluddite.com/tools/adding-tree-command-to-the-terminal-mac-osx) that worked perfectly. Simply open up your .zshrc file
and add this code:

    alias tree="find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'"

now return to the command line and run:

    source .zshrc

##That's It
Now, try it out! Simple type tree in the directory of your choosing and you're
good to go.
