+++
title = "Proxy Problems"
date = "2015-08-27T18:00:00+07:30"
tags = ["proxy", "firewall"]
draft = false
author = "Amitabh Das"
+++

Just completed the campus to corporate training at Oracle MySQL and ready to start! At the end of our training, we were given a task to complete to _“refresh our C++ skills”_.

We were to design a program which would simulate MySQL on a smaller scale – Multiple clients, shared data structures, locks and a few more features.

Of course I started off almost immediately but I came across a problem.

The Oracle office I worked in had a proxy in place. I would need to set the proxy at office and then when I'm back home, I would need to remove it again. And adding proxy in the system settings didn’t apply to my terminal for some reason so all apt-get’s git clone’s were failing.

**PAIN IN THE @$#!**
![image](https://78.media.tumblr.com/9c5b181774f7beb04c12ae84246516df/tumblr_inline_ntjnx2iHxH1tb0x0k_540.jpg)

So I wrote this tiny shell script which would detect the Wireless network you are connected to and set the proxy environment variable automatically.

Here is the link to the code: [https://github.com/amitab/auto-proxy](https://github.com/amitab/auto-proxy)

Of course, right now it uses only one proxy. But this can be easily extended to use different proxies for different networks.
