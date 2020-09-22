+++
title = "Git Gud"
date = "2015-12-07T18:00:00+07:30"
tags = ["git", "version management"]
draft = false
author = "Amitabh Das"
+++

After goofing around the last month, I suddenly had multiple bugs thrown at me. Well it came as a relief actually, since I was getting real bored of watching TV shows everyday.

# The Situation

Alright so these guys here have a different way of working on the code base than the way I previously experienced in my internship.

So we have a bug, and a main branch on to which our fix should go to. I create the fix and then create a patch to apply to the main branch. Working on multiple bugs means I need multiple branches. And make a patch with respect to the parent branch of this bug branch after I am done with the fix.

    git checkout -b [bug branch]
    git diff [original branch]..[bug branch] > patch.diff

# The Problem - So many bugs, so little time
![image](https://78.media.tumblr.com/efcb563b12d9b6796a9393add5726f8f/tumblr_inline_nxghvxfwDD1tb0x0k_540.jpg)

I have a bunch of local repositories that I am working on, with each of them forked at different points of commits. I wanted to find a way to keep a track of these commits so that patch generation is as simple as:

    g patch

# The Solution

The answer was `git-reflog`. `git-reflog`'s shows the change history of HEAD in your repo. Here is what I did:

    git reflog show --pretty=format:%H [bug_branch]

And the last entry of this output was the commit hash when the brach was first created! Now I just need to record this and find the diff between this commit hash and the current commit hash of my bug branch and we have a patch!

I needed to find a way to automate this process – to be able to execute all these commands in one go. On top of that I need to keep track of the original branch of the bug branch. So I wrote a bash script wrapper over git to automate this.

Here is the link: [git-wrapper](https://github.com/amitab/git-wrapper-bash)

# The End

Of course the commands I wrote were verbose, but I finally managed to get custom auto-complete working for my bash script, so just press tab and you got yourself a time-saving script for branch control and patch generation!

**EDIT:**
I've been researching some more on this git-reflog. Turns out this has an expiry time. This was unreliable so I had to ditch it. Currently I create a DAG of all the commits and try to pin-point the first commit of the branch. The updated code is in the repository.
