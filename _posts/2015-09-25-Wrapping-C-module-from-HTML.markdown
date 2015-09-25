---
layout: post
title:  "Calling C module with HTML form | Part 1"
date:   2015-09-25 23:03:06
categories: software, web, C
---

Looks like I've been away for a while. I have now discovered that it really _is_ hard to maintain a blog. Gone are the days when I make fun of other bloggers who aren't consistent in their bloggage. I need to be persistent. [Thanks, Jeff Atwood](http://blog.codinghorror.com/how-to-achieve-ultimate-blog-success-in-one-easy-step/).

I have a long list of post ideas I'd like to write about, but this is the one I've been working towards as of late.

Some of the work I'm doing now involves retrofitting an older C module with some new functionality and shipping it to a customer far away. The module itself wasn't too hard to modify, although retrofitting code can be hard if you don't have any unit tests to test against (more on that in a future post).

However, the customer doesn't like using a terminal. This is bad, since the executable itself can only be run with a terminal. This is also unusual for us, as our customers in the past have been more than able to use a terminal. User Experience isn’t something that’s high on the priority list for us. Having added the new functionality the day before flying out to deliver the system, I was pressed for time to add a gui component to appease the all-important customer.

And so, with what little time I had, I wrote a little bash script to wrap the executable with the command line arguments they were most likely to use, with some hooks for testing purposes. I also wrote another script to wrap the first wrapper, and with it introduced a config file format that consisted of keyword=value pairs. The format of the config file was simple, as the syntax conforms to a environment file that can be ``` source ```'d inside bash. More on that [here](http://wiki.bash-hackers.org/howto/conffile).

 The wrapper approach made things more simple, but I got carried away with my love of bash, and forgot the original problem still remained: the use of the terminal was a big no-no.

And so, with half the day already spent, I figured I’d use _zenity_ to add little information boxes, entry fields, and the like to the high-level wrapper. So rather than having to create a config file manually, the user can run the high-level wrapper, which will ask for all the parameters in the file and create it, in a Microsoft Office wizard-like fashion. It works. kinda. Not too pretty.

I’m perusing other simple options for making the user experience a bit smoother. Since I’m learning jQuery and a bunch of stuff about the web right now, I thought I’d try to do something along those lines.

As it stands right now, I’ll probably use CGI to call the script from a HTML form on a web page, capture the output and print it out on the web page.

A pretty graph to illustrate:

![Because flowcharts are baus](/assets/wrapper-first.png)


I was considering throwing out the bash scripting in between the form and the executable, but I’d like to be able to save the config file for future use, so it’ll stay.

Another entry point for importing a stored config file would make the overall flow look like:

![Because more complicated flowcharts are even bausier](/assets/wrapper-second.png)


Flowcharts always make everything more exciting!

UPDATE: As it turns out, the customer lost interest in the aforementioned mods a few months ago. I still like the idea of this mini project, and will attempt to complete it. Besides, it may prove useful in other contexts.

I know CGI is a bit old, but it seems to be the quickest way to get up and running.

That’s all for now for this project. Next post (which hopefully will come soon) in this series will dive into the setup and code required to make this project work.
