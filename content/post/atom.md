+++
title = "Adventures with Atom"
date = "2016-03-03T18:00:00+07:30"
tags = ["atom", "plugin"]
draft = false
author = "Amitabh Das"
+++

Quite recently I came across this new Text Editor by Github, called the Atom editor. Everything from its description to its UI was eye catching and I really wanted to give it a shot.

And...

# My thoughts

What I loved the most about atom was its community. They have got a plethora of packages, providing a host of features to improve and speed up your daily tasks. For example – They have got a [Git plugin](https://atom.io/packages/git-plus) (since it’s from Github) and with the right key bindings, you don’t ever have to look at a terminal! Except to compile maybe. And I say maybe because they’ve even got packages (this one I use: [atom-build](https://github.com/noseglid/atom-build)) which take a compile configuration and run the compilation by themselves!

# So… what was this adventure?

Well the code with which I work is written in C/C++. And I have to use cscope to navigate through the code base (and to keep my sanity). &nbsp;But I did **not** like the cscope browser UI! What I wanted was a way to keep the results list in one pane and select one of the results to open up the file in another pane. Hide the cscope pane when you are done. So that’s exactly what I did. You are Welcome. 

The initial stages were slightly difficult when you needed to learn what does what. But creating the package was surprisingly simple. I learnt coffeescript in the process (so similar to javascript).

# Shameless self promotion

So here’s my pet project – [atom-cscope](https://atom.io/packages/atom-cscope)

Feel free to raise any issues you find, on the [github](https://github.com/amitab/atom-cscope) repository page.
