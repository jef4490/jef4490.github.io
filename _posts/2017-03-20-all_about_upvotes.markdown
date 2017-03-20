---
layout: post
title:  "All About Upvotes"
date:   2017-03-19 23:28:16 -0400
---

This past week we deployed our first projects at Flatiron. The five day period we were given to create these web applications is referred to as “project mode” and it is an accurate term for my M.O. during that period. All structure in my life went out the window as I coded and coded; I would sometimes realize at 11 PM that I hadn’t eaten dinner. My teammate, Ian, noted that it was a rare situation where time, not creativity or ability, was the limiting factor in what we could produce. I cannot overstate what an awesome thing it was to be so involved in the work we were doing and the code we were writing.


There were a lot of lessons learned during project mode and I will probably go into some of them at a later point in this blog, but today I want to talk about the thing I am most proud of having implemented into our application: upvotes.

![](http://i1.kym-cdn.com/photos/images/original/000/675/823/6ab.gif)

Some context. Our project was an application that functioned as a music discovery platform. We utilized the Spotify API to encapsulate song objects that included an embedded player. The main focus of the app was that users would write reviews for songs they listened to and these reviews would determine recommendations for their friends. However, in an effort to further control for review quality and more intelligent recommendations, we wanted to implement an up upvote system that gave people the chance to indicate whether a particular review was good or helpful. This mechanic is, of course, implemented on lots of popular websites today in a variety of ways: Reddit upvotes/downvotes, Facebook likes, YouTube thumbs up/thumbs down. They all have slight differences (e.g. Facebook now has several types of reactions, some negative) but fundamentally allow the developers to gather information about how your users enjoyed particular content. This is really cool for two reasons. 


First, you can learn more about your the quality of your content than merely the frequency & quantity of views. Think how misleading the view count of [this video](https://www.youtube.com/watch?v=kfVsfOSbJY0) would be if we didn’t see the context of the thumbs down indicator. It’s important to know what’s popular for the right reasons. 

![](http://i.imgur.com/IyrCQJv.gif)

Second, you can also learn about the user’s individual preferences. Being able to curate content to match a particular user’s taste. For a music discovery platform like ours, the applications are pretty obvious. People tend to have strong preferences that do not always overlap with others’.

## Domain Modeling

So what’s holding us back? Well a few things. First, where do we keep and access this information? In our database, we can persist this information for future use and confirm that users aren’t able to upvote the same content multiple times. So it made sense for us to set this up as a join table, votes, with foreign keys to an account (user) and to the review that it applied to. We also had a column for the value of the vote (1, 0, or -1) which allowed us to use SQL aggregating to sum up these values for an individual review’s score. But when we began to set up our object model for votes and code the relationships it became clear it was a little more complicated than the examples we’d be exposed to so far. 

![](https://i.imgur.com/WnAXZZu.png)

In the case of a song object, the relationships are pretty cut and dry. Using ActiveRecord for our ORM, we employ macros to describe that a song belongs_to an artist, and also has_many reviews. We also tell a review that it belongs_to a song, and we tell an artist that it has_many songs (and many reviews through songs). The advantage is that we can ask a song who it’s artist is through the #song.artist method and it will return the artist object. So with that in mind, a vote belongs_to an account and a review, as defined in our table. A review has_many votes and so does an account. But an account has_many reviews and therefore has_many votes through reviews. So what do I want returned when I call #account.votes? Well, we really have two relationships- one where the account acts as the voter and another as the vote recipient. Even though votes are attached to a review, they are still important and relevant in the context of an account, who on our application, has the sum of our review’s upvotes displayed on their profile page. 

So how can facilitate the two relationships at once? We chose to use the has_many votes through reviews strategy. This means account.votes returns the collection of votes cast on their reviews. I then wrote several methods to approximate the second relationship, where an account may want to know more about the votes it has cast. 

![](https://i.imgur.com/8hcY9Cu.png)

Next question: how does a user cast a vote? A vote only makes sense within the context of the review it’s applied to. We originally concocted the up votes as radio buttons that were submitted through a form on the show partial for a review. But this was always temporary measure as we wanted to utilize thumbs up and down icons that the user could click to share how they felt about a review.

![](https://i.imgur.com/FuFH2IR.png)

The icons, however, needed to light up to indicate how you’d voted previously. Eventually, this meant reworking the system so that the icons were actually buttons that submitted separate forms with hidden fields containing values about the score of the vote cast. That is, clicking the thumbs up submitted a form to make an upvote whilst clicking the thumbs down button submitted a different form to create a down vote. 

![](https://i.imgur.com/csqXVf8.png)

When loading the show page for a review, we’d check to see if you’ve cast a vote for this particular review and if so, display the edit version of the form. Three main distinctions in this version. As you’d expect, the form now edits your existing vote. Second, the button corresponding to the score of your prior vote would be illuminated (using CSS). Finally, the illuminated button now dynamically changed it’s function to set your vote to neutral upon a second click. If I see these icons…

![](https://i.imgur.com/2UtEQO8.png)

…it should follow that by clicking the upvote button again, the light would turn off and the value of my vote would be zero, as if I had neither upvoted nor downvoted. 

## Real Time Response

I would have been satisfied with this system had Ian not offhandedly said, “If only we didn’t have to reload the page”. Yeah. That would be cool, but it was beyond my familiarity and abilities. Still, I couldn’t get the idea out of my head. My classmate Charles suggested [WebSockets](https://en.wikipedia.org/wiki/WebSocket), a topic I with which I was familiar in theory but less so in practice. So, thinking “am I really going to go down this rabbit hole?”, I embarked on the quest for real-time upvotes. I started with [this excellent blog post](https://blog.heroku.com/real_time_rails_implementing_WebSockets_in_rails_5_with_action_cable), which went through the steps to set up action cable, Rails’ built in web socket technology. The blog was fairly detailed, I needed only adjust a few pieces to make her solution work for my problem. And before I knew it, I was	 seeing this in my terminal

![](https://i.imgur.com/iDQH0WR.png)

What a rush. So, the infrastructure was in place- I could push data in real time across my application. But how did I actually get it to work for me? Well, it turns out I really needed to take a step back and learn a little bit about [Ajax](http://guides.rubyonrails.org/working_with_javascript_in_rails.html), specifically intercepting form submissions. With a form, upon submission, we are typically redirected to a new page or view or some sort. But to make this system work, I had to stop this from happening. It turns out to be pretty easy to do so. Just add a line like this to your html.

![](https://i.imgur.com/F1bCouX.png)

This tells Ajax to handle the controller action for us, which will result in our normal database behavior, but not a page redirect. Then, the WebSockets allow us to broadcast some of this new information back to the view for some dynamic visual changes. And that’s the key distinction. If you were to refresh the page, everything would reflect your most recent form submission as it does on every page load. But we can fake these visual changes in real time using Javascript, informed by information broadcast from the controller via WebSocket. These aesthetic updates were composed of three components. 

1. The icon illumination needed to be checked and updated.
2. The total number of votes cast needed to be incremented.
3. The hidden form attached to each icon needed to be changed to make the illuminated icon represent un-voting as before.

To start we began broadcasting the vote.score after being updating in the database and the review_id so we could make sure that these changes were applied to the correct partial.

![](https://i.imgur.com/m1Zjhkp.png)

The first of the three items was relatively simple to implement. The icons were colored with CSS and this was dependent on a class attribute. I could add a class to the particular HTML elements. 

The second item was a little trickier. What we ended up realizing was this required broadcasting an additional piece of information; previous score. This could inform whether we were going from neutral to a downvote or upvote to downvote, which would mean the difference between subtracting 1 or 2 from the vote total. Important distinction.

The third item was very much like the first, it meant editing the html of the form’s hidden field value.

All in all, the hardest part was actually writing the Javascript. Because I didn’t know Javascript. So I did my best. And got some help from teammate Frederico. 

So I would up with this.

![](https://i.imgur.com/GbpLuzR.png)

And this.

![](https://s15.postimg.org/d0b3l7fsr/ezgif_3_c9dbcaffaa.gif)

But also this. :-(

![](https://s13.postimg.org/9slqoh3on/ezgif_3_a25e597e78.gif)

Oh no! Even though their votes were distinct, the two users were looking at the same form. That’s not good. Let's try something like this to make sure their each getting a unique form (and broadcasting this id over the WebSocket as well).

![](https://i.imgur.com/uCUw2Uz.png)

Phew. At last, our work is complete!

![](https://s14.postimg.org/6ff8qekb5/ezgif_3_7794e948af.gif)

What I loved about this particular portion of the project was that I got the implementation of this feature through from concept to reality. I thank my teammates for trusting me to make it work, and let me go down the rabbit hole of WebSockets to emerge the better for it. I hope that this may be helpful to somebody looking to use this system in their own application. And I hope to have many more coding experiences as engaging as this one proved to be.

See this code in action at [KokoMusic](http://kokomusic.herokuapp.com). ([GitHub](https://github.com/depaolif/yelpify))
