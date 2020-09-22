+++
title = "Lost and Found"
date = "2016-09-22T18:00:00+07:30"
tags = ["bash", "script"]
draft = false
author = "Amitabh Das"
+++

So, last week, I was cleaning up my disk partition to free up some of the space all those auto-generated files, created by makefiles and other confidential stuff, took up. Then I open up a new terminal tab and suddenly I see this:
![image](https://i.imgur.com/mLdvTEK.png)

> _NOOOOOOOOOO!!! I lost all my utility scripts!! - Me_

This is it.

I have to actually write down the full commands now. At this point, I gave up on the bug I was working on and was about to go downstairs for a cup of coffee.

While I was sipping on my warm coffee, trying to make a mental note of each of the aliases and functions I wrote, I realized one thing. 

The terminal I used while cleaning the disk was still open.

# What does that mean? 

I'll tell you what it means. It means that my scripts are still loaded there! I just needed to find a way to get to it.
A quick little google search pointed me to the declare method.

You want your functions back? Here you go:

    declare -f

You erased your variables? Use this:

    declare -x

You want to recover your aliases? You will need this:

    alias

# What did I learn from this? 

Guys, always back up your scripts. Always!
