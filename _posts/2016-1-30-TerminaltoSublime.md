---
layout: post
title: Opening Sublime from Terminal
---


##Opening a file from the Terminal to Sublime 3


Here I will show you a handy trick where you can open a file in Sublime from the terminal. You need to run a few commands to setup the functionality in command. 

###Step One

Find the needed path by entering the "echo $PATH" command in your terminal, like so:

![an image alt text]({{ site.baseurl }}/images/CommandLine1.png "an image title")

You will be using the path at the end of the result, mine will be "/usr/local/git/bin"

###Step Two

Enter the command:

ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" 

(then your file path) /usr/local/git/bin

(then sublime) /sublime

It looks like the following:

![an image alt text]({{ site.baseurl }}/images/CommandLine2.png "an image title")

###Step Three

Now when you navigate in the terminal to a file or folder you want to open in Sublime, simple type

"Sublime ." in the terminal, and your file will magically open in Sublime!! 



