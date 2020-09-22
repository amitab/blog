+++
title = "Versions of madness"
date = "2017-03-17T18:00:00+07:30"
tags = ["version management", "virtualenv", "python"]
draft = false
author = "Amitabh Das"
+++

Does your product support multiple versions of a particular language? Older versions of that language? (I feel you bruh)

![image](https://78.media.tumblr.com/281ffe5d42a8941c1c038cd23424e55c/tumblr_inline_ogaeo71dJD1tb0x0k_540.png)

I work on a product which supports multiple versions of python. Now don’t get me wrong, python is an amazing language. Currently, we are supporting 2.7.x and 3.3.x.

Like that wasn’t enough, there are a few hair-ripping changes from versions &lt; 2.7.9 which I discovered recently. SSL is a total wreck in 2.7.6! And I was given the job to include SSL support.

Had very little time to do it. Quickly looked up about SSL and found out about the SSLContext in python. Then the genius me decided to not read the Documentation (_Don’t ever do this_). But hey, it was all working neat with SSLContext objects (from &gt; 2.7.9). I didn’t even consider checking 2.7.6 because, who adds a major feature in a micro release, right?

Well one bright day, I was chilling at the cafeteria, drinking my favourite – Arabian Pulpy grape juice when I got this email on my phone. (Why have I synced my work emails with the phone? Why haven’t you?) It was from the QA guys who sent a mail saying that the entire product was not working.

_What._

I mean I ran **all** the tests! 2.7 and 3!

Then the genius me decided to look at the Documentation after all this shit. I mean, who even. I see the Conditions Apply text at the bottom stating that SSLContext was only available &gt; 2.7.9.

Okay, my bad. There were two ways I could have avoided this fiasco.

1. Read the Documentation. **Always**.

2. Test in every version of python.

_Test in every version of python you say? But how can one install every micro version of python 2.7?_

Worry not my crestfallen children. I shall reveal this to ye.

[https://github.com/yyuu/pyenv](https://github.com/yyuu/pyenv)&nbsp;is what I should have had long ago. This magnificent tool, sent by the gods help the everyday python developer to deal with testing code in multiple versions of python.

The installation instructions are pretty straightforward. Once you have it up and running, I suggest you write a script that invokes your test framework each time with a different python version selected.
