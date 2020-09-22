+++
title = "Testing, Testing... 1, 2, 3"
date = "2016-04-19T18:00:00+07:30"
tags = ["testing", "environment"]
draft = false
author = "Amitabh Das"
+++

> _I am a developer. I don’t care about testing._ – The young developer

Of course it starts like that. And right when you are presenting your project in front of others, you realize that the feature isn’t working. My god, what a terrifying situation.

Then you realize the importance of testing. You find out about the different types of testing, and how you wish that you had written tests for each feature/fix you committed so that the problem would have been caught immediately. But no matter, you can think of this as a learning experience and make sure you get it right next time.

# My Story

This happened a week back, when I wrote and released a feature requested for one of my pet projects ([atom-cscope](https://www.github.com/amitab/atom-cscope)). It worked perfectly fine on my computer so I didn’t really care. After all it’s just something I was doing on the side, so I didn’t dig deep. Soon after the release, the person who requested for the feature raised an issue saying that it wasn’t working.

Huh? It was working perfectly fine on my system.

I went through the feature commit line by line. And then I realized I did not declare a variable and was using it for an important operation. And it was working on my computer! How? It is possible that some other package had declared this variable **WITH THE SAME NAME, USING THE SAME LIBRARY** (what are the odds -_-) globally somewhere else which is why I was able to use it.

# Conclusion

I learnt an important lesson – always test in isolation. The feature requester also suggested that I do my testing in a clean Virtual Machine, which I think is a good idea.
