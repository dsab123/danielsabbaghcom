---
layout: post
title:  "My First Web Project"
date:   2015-07-25 23:03:06
categories: playhymns, software, web
---

I suppose it's fitting that my first post with any real substance should bear a title with as trite a composition such as "My First *".

Well, this post was a long time coming. Now that I've (somewhat) finished my first real project involving the internets, its time to blog about it.

The URL of the project can be found [here](http://playhymns.herokuapp.com), and the code is hosted on github [here](https://github.com/dsab123/PlayHymn3).

I've named the project PlayHymns, because I'm great at naming things like that.

I'll discuss the need from which the desire for this project came forth, a detailed description of the project structure, what worked well, what didn't, what I learned, and what I want to learn or improve upon as a result of finishing the project.

##Need

I attend Trinity Reformed Church of Gaithersburg, an awesome new church plant in Maryland.

Our church worships the Lord in song with traditional hymns, because of the [regulative principle](https://en.wikipedia.org/wiki/Regulative_principle_of_worship).

Many of us, however, have not been sufficiently exposed to the style and nature of hymns, and it has been a struggle for us to collectively join our voices together in song.

As the church pianist, I am responsible for learning the songs and leading the worship.

I decided that it would be beneficial for our church to have a resource from which they can acquaint themselves to the upcoming service's songs a few days beforehand.

I was emailing links to the tunes and lyrics of the hymns for each service for a while, but there were cases in which I couldn't find the tunes for some of the lesser-known hymns, and I wanted members to be able to access the hymns from previous weeks.

And so, the desire for this project was born.

I should mention right at the outset that this project was developed with a spirit of curiosity rather than necessity. This is my first web project, and in an effort to push past the necessary threshold of shame associated with learning a new skill (picture the scrawny guy who must overcome the fear of looking like an idiot when he begins going to the gym), I didn't aim for perfection on every aspect.

You have been forewarned. 

##All the Moving Parts



There are quite a few different technologies employed in PlayHymns, and so it seems best to examine them individually.

#Heroku

This is the first time I've ever hosted a project on heroku. It was fairly straightforward. You can push the code from git to a *dyno*, which is a linux container that's responsible for running your app. Pretty cool.

The project's final form is a runnable fat jar, and so it was very easy to start the app by running a single command, which you tell heroku to run by putting it in your Procfile:

```
web: java -Dserver.port=$PORT -jar target/PlayHymn3-0.1.0.jar
```

One of my aims for this project was a persistent way for members of my church to access previous weeks' hymns, and that just screams 'database,' doesn't it?

I am most familiar with SQL, and as it turns out, there is a heroku addon called [ClearDB](https://devcenter.heroku.com/articles/cleardb). ClearDB setup was fairly simple to hook into the rest of the project.


#Spring

This was also my first time using Spring. I was concurrently reading [Spring In Action](http://www.manning.com/walls4/) while developing PlayHymns.

To interact with the 



#CORS












