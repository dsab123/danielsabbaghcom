---
layout: post
title:  "My First Web Project | Part 3"
date:   2015-10-29 06:32:00
tags: playhymns software web
---

So, in my last [post]({% post_url 2015-10-22-I-Need-To-Become-A-Fundamentalist %}) I decided that I was going to update all the aspects of PlayHymns: the database, the UI, the front end, the back end, and the filesystem. And I wanted to learn QUnit to be able to write unit tests for the JavaScript.

Today, I made some progress with the UI and front end. Namely, I used the [Mustache.js](https://mustache.github.io/) templating library to make my codes better.

I came across a [really long blog post](http://openmymind.net/2012/5/30/Client-Side-vs-Server-Side-Rendering/) about server and client-side rendering, and came across this quote:

>With client-side rendering, your initial request loads the page layout, CSS and JavaScript. It's all common except that some or all of the content isn't included. Instead, the JavaScript makes another request, gets a response (likely in JSON), and generates the appropriate HTML (likely using a templating library).

"Hmm," I thought to myself, "what is this templating library of which he speaks?"

So I did some googling, and found Mustache.js. It's pretty cool, I guess.

Here's a gist that explains how to do this.

In my js, I perform a get request from the server that returns a list of all the weeks for which I've uploaded hymns. The file below demonstrates how I placed this week inside a manually-created anchor and append it to the dropdown list.

{% gist b6662dc9e3c4a2e1f713 without_templating.js %}

Borrowing heavily from [this post](http://www.sitepoint.com/creating-html-templates-with-mustachejs/), I moved the html to its own file, separating it from the javascript.

{% gist b6662dc9e3c4a2e1f713 template.html %}

Modularity!

In the javascript, I added another get request to fetch the template I wanted (filtering on the id), and used Mustache to template the variables into the html.

{% gist b6662dc9e3c4a2e1f713 with_templating.js %}

As you can see in the comment in with_templating.js, I had to add the index to the data returned from the server in order to get it to be templated.

Under the heavy traffic my site sees on a daily basis, I'll do some analysis to see if its really faster or not. It definitely seems more readable though!