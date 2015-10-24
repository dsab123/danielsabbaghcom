---
layout: post
title:  "My First Web Project | Part 2"
date:   2015-10-07 23:03:06
tags: playhymns software web
---

My apologies for waiting more than three months to continue this series! I'm glad to see that I haven't lost any member in my exhorbitantly large fanbase because of my slackage. 


In reviewing the first part of this series, I realized I never gave a high-level overview of the project. So, I’ll begin this post with just that.

Here’s a really neat flowchart that illustrates the project flow:
![I think I'm really starting to get the hang of these flowcharts](/assets/playhymns-flowchart.png)



We discussed the database structure in the first post of this series, and we said that there were two tables: ``` hymn ``` and ```week``` . The ``` hymn ``` table stores the number, name, lyrics, mp3URI and oggURI of a hymn, the last three of which are URLs into the S3 filesystem, and the first of which is the primary key. The ``` week ``` table stores the week (in MM-DD-YYYY format) and the three numbers of the hymns that were sung in that particular week.

It’s important to note that the jQuery can request either a week or a hymn; as such, the steps can refer to either. As it turns out, the only difference is that for the week, no requests to the filesystem are performed by jQuery, since all that is returned is the number of the hymn. I’ll go into more detail on this in the explanation of steps 3, 4, and 6.

Here's a nominal use case for a user clicking on hymn ``` 53 ```:

1. User clicks on the ``` <div> ``` containing the number 53 for a given week (if you’ve looked at the website, you’ll notice that you can’t click on any hymns in the beginning; I’ll explain why once you’re familiar with the whole project flow).

2. jQuery has registered an click handler for that hymn, which runs a GET request on ``` playhymns.herokuapp.com/hymn/53 ```

3. The Spring backend is expecting this, as it accepts GET requests on the ‘hymn’ endpoint. Using some awesome magic in JPA, Spring queries the MySQL DB for an entry in the ``` hymn ``` table with primary key 53.

4. The DB returns the info on that table to Spring. Simple enough.

5. Spring repackages the info on hymn 53 into JSON, and returns the JSON structure to jQuery

  * **remember, the lyrics, mp3URI, and oggURI fields are actually URLs into the S3 filesystem**

6. jQuery populates the divs for the simple fields of the hymn (name and number),

7. ``` load() ```s the lyrics and mp3URI resources from the S3 bucket

8. into their respective locations in the HTML.

This should probably be the first post in the series, eh?

Unfortunately, that’s all I have time for right now.

Next post will fill in any missing details.

Note to self: **DOCUMENT PROJECTS AS I AM DOING THEM**. It took me way too long to remember how all these parts interacted with each other!

Another note: the URL of the project can be found [here](http://playhymns.herokuapp.com), and the code is hosted on github [here](https://github.com/dsab123/PlayHymn3).
