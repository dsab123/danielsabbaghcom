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

I attend [Trinity Reformed Church of Gaithersburg](http://www.trcofg.org), an awesome new church plant in Maryland.

Our church worships the Lord in song with traditional hymns, because of the [regulative principle](https://en.wikipedia.org/wiki/Regulative_principle_of_worship).

Many of us, however, have not been sufficiently exposed to the style and nature of hymns, and it has been a struggle for us to collectively join our voices together in song.

As the church pianist, I am responsible for learning the songs and leading the worship.

I decided that it would be beneficial for our church to have a resource from which they can acquaint themselves to the upcoming service's songs a few days beforehand.

I was emailing links to the tunes and lyrics of the hymns for each service for a while, but there were cases in which I couldn't find the tunes for some of the lesser-known hymns, and I wanted members to be able to access the hymns from previous weeks.

And so, the desire for this project was born.

I should mention right at the outset that this project was developed with a spirit of curiosity rather than necessity. This is my first web project, and in an effort to push past the necessary threshold of shame associated with learning a new skill (picture the scrawny guy who must overcome the fear of looking like an idiot when he begins going to the gym), I didn't aim for perfection on every aspect, and many parts of the project are fairly scrappy.

You have been forewarned. 

###All the Moving Parts

There are quite a few different technologies employed in PlayHymns, and so it seems best to examine them individually.

##Heroku

This is the first time I've ever hosted a project on heroku. It was fairly straightforward. You can push the code from git to a *dyno*, which is a linux container that's responsible for running your app. Pretty cool.

The project's final form is a runnable fat jar, and so it was very easy to start the app by running a single command, which you tell heroku to run by putting it in your Procfile:

```
web: java -Dserver.port=$PORT -jar target/PlayHymn3-0.1.0.jar
```

One of my aims for this project was a persistent way for members of my church to access previous weeks' hymns, and that just screams 'database,' doesn't it?

I am most familiar with SQL, and as it turns out, there is a heroku addon called [ClearDB](https://devcenter.heroku.com/articles/cleardb). ClearDB setup was fairly simple to hook into the rest of the project.


##Spring

This was also my first time using Spring. I was concurrently reading [*Spring In Action*](http://www.manning.com/walls4/) while developing PlayHymns.

The last chapter of *Spring In Action* mentioned the [Spring Boot Project](http://projects.spring.io/spring-boot/), a great little convention-over-configuration framework that consolidates many standalone Spring entities into one project by lumping them all together in one POM. So once I got the POM working, I didn't have to hunt around for any more dependencies.


Some relevant sections of the POM:

```
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.2.2.RELEASE</version>
	</parent>
```

Including the starter parent POM was the easiest configuration option.


```
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>

	<!-- Testing Dependencies -->
		<dependency>
			<groupId>org.mockito</groupId>
			<artifactId>mockito-all</artifactId>
			<version>1.9.5</version>
		</dependency>
		<dependency>
			<groupId>pl.pragmatists</groupId>
			<artifactId>JUnitParams</artifactId>
			<version>1.0.2</version>
		</dependency>
		<dependency>
			<groupId>eu.codearte.catch-exception</groupId>
			<artifactId>catch-exception</artifactId>
			<version>1.3.4</version>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.seleniumhq.selenium</groupId>
			<artifactId>selenium-java</artifactId>
			<version>2.45.0</version>
		</dependency>
		<dependency>
			<groupId>xml-apis</groupId>
			<artifactId>xml-apis</artifactId>
			<version>1.4.01</version>
		</dependency>
	</dependencies>
```

And here are the other dependencies I added to the project. Most of these were for playing around with Unit Testing...

#JPA

This was also my first time working with the Java Persistence API (henceforth JPA). It was surprisingly easy to work with. For most people, I'm sure this is old hat and so I won't go into too much detail.

For reference, here are the snippets of the two tables used in PlayHymns:

Week table sample
```
+------------+-------+-------+-------+
| Date       | hymn1 | hymn2 | hymn3 |
+------------+-------+-------+-------+
| 03-31-2015 |    53 |   634 |   642 |
......................................
| 07-26-2015 |   309 |   646 |   369 |
+------------+-------+-------+-------+
```

Hymn table sample
```
+--------+----------------------+------------------------------+------------------------------+------------------------------+
| Number | Name                 | Lyrics                       |  mp3Uri                      | oggUri                       |
+--------+----------------------+------------------------------+------------------------------+------------------------------+
|     53 | Praise To The Lord   | https://s3.amazonaws.com/... | https://s3.amazonaws.com/... | https://s3.amazonaws.com/... |
..............................................................................................................................
|    634 | Sweet Hour of Prayer | https://s3.amazonaws.com/... | https://s3.amazonaws.com/... | https://s3.amazonaws.com/... |
+--------+----------------------+------------------------------+------------------------------+------------------------------+
```

The `Week` table holds the date of each service along with the three hymns that were sung. Because Old-Style Presbyterians only sing 3 hymns per service (NOTE: this does not include the doxology). 
 
The `Hymn` table stores the Number (as the primary key), Name, lyrics, and a URL for both mp3 and ogg recordings of the hymn.

I played the hymns on my keyboard at home, and recorded them on my Galaxy S5. Hence the clear, crisp audio.

With Spring and the JPA, you can create a simple Java Bean class with instance variables that match columns of a table in your database. For many people this is probably old hat, since the first version of JPA came out in Java 1.6, but you were probably surprised when you used it for the first time, weren't you?

Here are the java classes that interfaced with the `Hymn` table:


```
Hymn.java 

@Entity
@Table(name="hymn")
public class Hymn {

	@Id
	private int number;
	
	@Column(nullable = false)
	private String name;
	
	@Column(nullable = false)
	private String lyrics;
	
	@Column(nullable = false)
	private String mp3Uri;
	
	@Column(nullable = false)
	private String oggUri;
	
	public Hymn(String nameIn, String lyricsIn, int numberIn, String mp3UriIn, String oggUriIn) {
		name = nameIn;
		lyrics = lyricsIn;
		number = numberIn;
		mp3Uri = mp3UriIn;
		oggUri = oggUriIn;
	}

...
```

And by adding a Spring Controller to the mix, you can generate queries to your table on the fly. Mind=blown.

HymnRepository.java
```
public interface HymnRepository extends Repository<Hymn, Integer> {

	Page<Hymn> findAll(Pageable pageable);
	
	Hymn findHymnByNumber(Integer number);
}
```


HymnController.java
```
@RestController
@Configuration
@RequestMapping("/hymn")
public class HymnController {
	HymnRepository repo;
	
	@RequestMapping(value="/{idNum}", method=RequestMethod.GET)
	public @ResponseBody Hymn returnHymn(@PathVariable int idNum) {
		return repo.findHymnByNumber(idNum);
	}
	
	@Autowired
	public void setHymnRepository(HymnRepository repoIn) {
		repo = repoIn;
	}
}
```

The `returnHymn` method maps the `/<hymn number>` endpoint on the app to a call to the `Hymn` repository, which queries the `Hymn` table for the given hymn.

A similar scheme was used for querying the `Week` table.

Unfortunately, I'm out of time. 

Also, that snippet of the tables probably doesn't look all that pretty. I'll need to get better at markdown to figure out how to wrap the lines better.

I'll discuss the database structure in more detail in the next post, as well as the rest of the discussion forecasted at the beginning of this post.


Until next time!

