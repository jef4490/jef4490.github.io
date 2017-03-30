---
layout: post
title:  "Make it Fast & Make it Work"
date:   2017-03-29 20:42:01 -0400
---

A few weeks ago, I wrote one of my first posts on the subject of [“Make it Work, Make it Good, Make it Fast”](http://jeffreyhechler.com/2017/02/19/make_it_work_make_it_good_make_it_fast/). At the end of the post, I mentioned how I wasn’t yet at the point in my journey to discuss “make it fast”. Today, I can add my updated thoughts on the matter informed by recent experience.

As mentioned in my [previous post](http://jeffreyhechler.com/2017/03/20/all_about_upvotes/), we recently completed our first projects at the Flatiron School, building a web application in Rails. Our group of three built a music discovery platform, leveraging the Spotify API to deliver playable music content to our users, but primarily facilitating user reviews and upvotes in order to inform personalized recommendations. There was a lot to be proud of but where it came time to demo our product, disaster struck. My teammates and I showed off the home page, performed a quick logged-out search, but when logging in to our previously created account, the client took too long to load the page and timed out. What?! We had been on the webpage just minutes before and it was working exactly as expected. Frantically, we switched computers and created a new user but this only exacerbated the situation. Then, total failure to load all pages. 

![Bummer](https://media.giphy.com/media/KRuu5c6Vdhd0A/giphy.gif)

The frustration of the moment passed and after explaining our application as best we could to our peers, my teammates and I gathered for a post mortem & debriefing. As we put our heads together, we concluded that the variable between our testing minutes prior and the demo itself were our peers in the room. Peers with laptops who, when shown the url of our application, may have visited the web page in greater numbers than we had tested with. Even if 5 or 6 new users had gone to the web page as my team was demoing, this may have greatly increased the stress on our backend, knowing that we also had three personal accounts logged in for the demonstration. 

If you’re thinking that any web app worth its weight should be able to handle < 10 users at the same time, you’re not wrong. As we went through our code, we discovered that the very thing that our app relied on most was what ultimately caused the failures in our reveal. Our app’s defining feature was suggesting new music to our users. This took a few different forms: trending songs, top voted reviewer’s picks, what your friends are listening to, etc. These all relied on retrieving data from the database that matched a particular criteria, sorting it, and displaying this in a way that was helpful for a user. We built formulas to determine what data matched our qualifications for a recommendation and enacted them in something like this:

```
def index
     # binding.pry
     # artists = Artist.joins(songs: :reviews)
     songs = Song.includes(:reviews)
     @trending_artists = {}
     # artists.each do |artist|
       songs.each do |song|
         # Song.where("weighted_review_average > 3").includes(:artist)
         if song.reviews.count > 0 && (song.weighted_average.round(2) > 3)
           if song.reviews.any?{|review| review.created_at > 2.weeks.ago}
             @trending_artists[song.artist] = song.weighted_average.round(2)
           end
         end
       # end
     end
     # Artist.joins(:songs).where("songs.weighted_review_average > 3").order("songs.weighted_review_average")
     @trending_artists = Hash[@trending_artists.sort_by{|k, v| v}.reverse]
 end
```

This probably makes most programmers a little sick to their stomach and for good reason- we’ll get into that. I will mention here that my teammate, Ian, wrote a [blog post](https://ianboynton.tumblr.com/post/158495373349/learning-from-failure) on this subject as well which is pretty insightful . I do, however, need to state that I reject the premise that the fault was his. It was a team project, and just as the successes were shared, so too were the failures. Every time we suffered through the noticeably long load times for our trending songs view and chose not to optimize it, we made the choice that this was not important enough of a problem to address, a mistake I’m unlikely to repeat.

Back to the code. Those familiar with Big O notation can see the inefficiency a mile a way. The method begins by pulling all accounts from the database and then iterating over all of those accounts to add the account object and its vote_total to a hash. Then we go on to sort the hash and iterate over all the keys. And so on. These are all large and expensive operations with respect to resources and we end up with an operation that is polynomial in complexity. What we really needed to do was dig in, get comfortable with ActiveRecord/SQL, and write code like this.

```
 def self.trending_artists
     Artist.select("artists.*, AVG(weighted_score)").joins(songs: :reviews)
	.group("artists.id").having('AVG(weighted_score) > ?', 3).order('AVG(weighted_score) DESC')
end
```

In the days that followed, we cleaned the rest of our inefficient code and the improvements were gargantuan. So what were the take aways?

1: Sometimes Make it Fast is the Same as Make It Work. 

How silly that I fell into the pitfall I discussed in my first blog post. In this case, failure to make the code fast meant total application failure. 

2: Failure is a part of coding.

It’s important to me to make sure that the experience of our demo blowing up becomes a learning opportunity. Composure has always been something I’ve prided myself on, so being able to stay in the moment and make the most of our presentation didn’t feel like too much of a stretch, but I’m glad to have been able to take the time to reflect on what happened and how to avoid it in the future.

3: Code is rarely finished. 

As we went back over the culprit methods and functions, we noticed other things that wanted some love as well. Simple things like commenting and proper indentation, but also greater consistency in variable names, for example.

In the end, it was a pretty humbling experience having your work blow up in your face in the moment. But I can also safely say that I will this was an error I am not likely to repeat and for that, I am grateful that the experience was in as warm and receptive an environment as it was. Now I dive headfirst back into the world of learning & code, wiser for the experience.
