---
layout: post
title:  "Make it Work, Make it Good, Make it Fast"
date:   2017-02-18 23:00:32 -0500
---


In 2013 I began working at a small music house in midtown Manhattan; a business that writes and records music for commercials, television, and occasionally movies. They specialize in creating music for picture and have developed a reputation for doing some really interesting stuff. Prior to working there I had a background in composition, but during my time at Analogue Muse I picked up a lot about audio engineering/production as well. I eventually went on to engineer the recording of a full length album with a [band](https://blackbeartribe.bandcamp.com/album/spin-measure-cut) I was in at the time. 
What does this have to do with coding? It’s the most comparable life experience to my current situation I’ve had- a need to absorb a massive amount of information in a relatively short amount of time. Learning each has been a privilege and a challenge, overwhelming at times. But I think there’s quite a lot of similarity in the process, both of which require a combination of technical ability and inspired creative thinking. A union of left brain and right brain. 

![](https://upload.wikimedia.org/wikipedia/en/6/6c/Rush_Hemispheres.jpg)

One of the first things I remember our instructor at Flatiron conveying to us were the words, [“Make it work, Make it good, Make it fast.”](http://wiki.c2.com/?MakeItWorkMakeItRightMakeItFast) The idea is that, when writing code, we can get so bogged down with the idea of writing the best code possible and this prevents us from actually solving the problems. I’ve certainly been there a number of times in the last week. The mantra suggests that it is better to write a functional solution to the problem, however dirty and code-smelly, and then turn one’s attention to refactoring the code, favoring more powerful enumerators and higher level functions. Perhaps writing more helper methods to make your code more readable and expressive. Having a working solution can aid in all of that. If my solution took five steps to complete the task, being able to see the return value of step four is immensely helpful in building better code. Maybe I can compact steps 1-3 into a single method and eliminate ten lines of code? Maybe I realize that one of the steps isn’t necessary at all. Either way, I’ve come to see the value in such an approach when dealing with challenging problems. 

![](https://i.imgur.com/BKF6uVq.png)
*Pictured above: refactoring [boat code](https://github.com/jef4490/model-class-methods-lab-web-0217)*


This sentiment is paralleled in recording music. Imagine you’re the drummer of a band in the studio and you’re asked to sit down alone and, in one take, give the best ever performance of your band’s song. You’re not really being setup for success. For one, you’re probably so focused on the form of the song (“How long are the verses?”, “Is this chorus the one where I drop out for a measure?”) that you can’t focus on nailing all of the details that make your track come alive. There’s a good chance you’re playing to a metronome which is also an unnatural situation. 

![](http://i.imgur.com/QoVt8lV.jpg)
*A drum set setup for success*

It’s not too much of a stretch to say you’d probably want to play along with a ‘scratch track’, a simple recording that some or all of the other members of the band made of their own parts for the sole purpose of giving you something to aid in your recording. You can now play that sweet fill you came up with synchronized with a tasty lick from the bass player knowing with 100% certainty that this is the right part of the song to do so. Then when you’ve delivered the hottest take you had, your band members ditch the scratch track and record their best performances to yours, reacting to the moments of inspiration you improvised into your recording. Maybe the guitar player heard that sweet moment between you and the bass player and dropped out to let it breathe. No. Probably not. But hey, you can dream. 

Make it work is the scratch track. It communicates the idea of the song to the listener. Make it good is the final take. 

I’m not yet at the point in my development journey where I can speak about making it fast. We’ve talked a little bit about ActiveRecord’s #join vs #includes and how this impacts performance by adjusting the way SQL database requests are handled. [For the curious](http://tomdallimore.com/blog/includes-vs-joins-in-rails-when-and-where/). That is the extent of my experience at this moment. That said, I think its parallel in recording is the production of a song. What tools we use to filter, to control dynamics, to add reverb and space to a recording. We go over our tracks and use effects processing to make sure that it’s playing nicely with the other instruments. We carve out frequency ranges to dedicate to our bass and kick drum tracks, adding rhythmic definition. Finally we master a track and make sure it meets the expectations we have of recordings. Is it the right volume? Does it sound like other songs of its genre? In making our code fast, I’d imagine we’re doing much the same, that is optimizing our code to meet our expectations of similar apps. Does it run quickly? Does the performance impact the experience?

![](http://i.imgur.com/Tzb9Unf.jpg)
*Make it work*

One thing that my research into this mantra revealed is the idea that the three have to be taken as a singular task. That is, we don’t make it right or make it fast when we have time for it, but rather our approach to putting code on the [skeuomorphic](https://en.wikipedia.org/wiki/Skeuomorph) page is a three pronged effort. The job isn’t done until we’ve taken time to complete the third step of ‘make it fast’. In the same way we wouldn’t publish our song after the scratch recordings are done, we don’t deploy software to merely works. 

Finally, it’s clear to me that there’s a balance to be struck in the make it good stage. While one can be tempted to write a single line of code with nested queries and enumerators, one is sacrificing readability and flexibility in debugging. We don’t want our drummer to overplay because we’ll start to lose the song. There’s such a thing as being [too hip](https://www.youtube.com/watch?v=ItZyaOlrb7E). It can be difficult for others to work with your code if it’s unclear exactly what is happening. Variable naming is a big part of this; making code expressive. Furthermore, following style guides and adhering to convention is key. Good doesn’t necessarily mean as small as possible in coding either, and there’s value to breaking things down for clarity. The Single Responsibility Principle. 
	
In music we have the concept of taste. A musician who knows when to dial it back because it’s not their moment is going to get more work than somebody who needs to show off. If you’re distracting from the lead vocal, you’re doing something wrong. I think there’s a concept of taste in programming too and likewise, it is very much about balance. Not that I have the chops to “overplay” as a coder. But maybe if I sit down everyday, work on my figurative rudiments, and learn a few coding fills by the best cats out there, I’ll be able to lay down a sweet coding solo someday.

Sources: 

[http://wiki.c2.com/?MakeItWorkMakeItRightMakeItFast](http://wiki.c2.com/?MakeItWorkMakeItRightMakeItFast)

[http://henriquebastos.net/the-make-it-work-make-it-right-make-it-fast-misconception/](http://henriquebastos.net/the-make-it-work-make-it-right-make-it-fast-misconception/)



