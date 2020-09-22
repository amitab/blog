+++
title = "Cannot reproduce"
date = "2017-01-02T18:00:00+07:30"
tags = ["testing"]
draft = false
author = "Amitabh Das"
+++

So the QA is like: _We cannot sign off on the release of the product cuz of these bugs!_
And the Dev is all like: _I cannot reproduce these bugs!_

Must be a Deja vu for many.

We don't know who or what is wrong. It could be an anomaly on the Dev's environment or in the QA's environment. Safe to say, we not only need to reproduce the code for the bug, we also need to be able to reproduce the same environment to locate the actual reason for these bugs.

Sooo... Schrodinger's bug? We won't know unless we open up the VirtualBox! See what I did there? No? Okay, I'll get out.

We won't have to look far in the internet to find Virtual Machine Emulators. I will be working with one of the most popular ones - Oracle's VirtualBox.
Out of the box (pun not intended), it's not very easy to work with. I had to face a lot of issues with port forwarding, setting up scripts to download necessary software, etc. This is where Vagrant comes in.

# What's Vagrant?

Vagrant is not a Virtual Machine, but a manager of sorts. It requires a Virtual Machine provider like the Oracle's Virtual Box. It provides a much needed abstraction over the setting up and management of a Virtual Machine. I mean, when you are on a deadline and you want to test the issue real soon, you don't want to get arrested on an issue like - WTF! I can't access the internet from the Virtual Box!

Really, all I had to do was:

    vagrant add box ubuntu/trusty64
    vagrant init ubuntu/trusty64 # A buch of virtual boxes are catalouged in the vagrant's website
    vagrant up

That's all there is to it!

To setup the box with required software, you just need to edit the Vagrantfile that appears after you run vagrant init to include the commands you want to run to setup the machine.

    config.vm.provision "shell", inline: <<-SHELL
       apt-get update
       apt-get install -y python-pip
       pip install protobuf
    SHELL

Plus, if you mess up the environment real bad, you could just hit the reset button!
    
    vagrant destroy
    vagrant up

I rate it a full 5/7. Really dank tool for devs and testers alike.
