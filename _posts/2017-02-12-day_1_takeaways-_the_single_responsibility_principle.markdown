---
layout: post
title:  "Day 1 Takeaways- The Single Responsibility Principle"
date:   2017-02-12 16:44:33 +0000
---

During my first day at Flatiron, we revisited an assignment, ‘Hashketball’, from the prework that involved digging into hashes to find specific pieces of information. I had done this particular assignment twice already (for the sake of review) so I wasn’t stoked about doing it again. That was silly. Here’s a list of things I learned from completing it a third time with a pro:

Learn #1: There’s more than one way to the right answer. When I solved these problems on my own, my logic generally consisted of iterating through a hash until a particular value matched my supplied argument, and then returning a different value from the same hash. It made sense to practice iteration as that was what we had been learning during that section of the prework. You could write most of my methods in pseudocode as:

```
attribute_method (player_name)
	iterate through large_hash
		if name == name
			return [name][attribute]
		end
	end
end
```

This is functional code. It performs the task it’s designed to do and does so with little resistance. But that was not how our instructor solved it, and he convinced us it wasn’t a great way to manage the data. “If you’re using #each,” he said, “you’re probably doing something wrong.” Yikes.

Learn #2: Delegation. That is, employing a helper method (or a buddy method as Ben calls it). This was a concept I was familiar with from previous work I’d done. That is, writing a second method that does one portion of a job and then calling it during the main method. In example: say I needed to write a method, #next_prime, to determine the next prime number following a supplied integer argument. I might create a helper method, #prime?, to determine if a supplied number is prime or not. Then in the body of my main method, I can just call this helper method as needed, rather than write out the test for primeness. 
This serves two purposes. First, it makes the code look a lot neater. It’s always nice to write just a few sentences that accomplish the job of an essay. It’s going to also be easier for another programmer to take a look and follow what’s going on. But, aesthetics and readability aside, it means the main method is really just the logic of the core problem and the programmer can look there for debugging. One can also more easily debug the individual parts of the problem by use of ‘pry’ to isolate issues. 

```
def next_prime(num)
  num += 1
  loop do
    return num if prime?(num)
    num += 1
  end
end

def prime?(int)
  return false if int < 2
  i = 2
  while i <= Math.sqrt(int)
    return false if int%i == 0
    i += 1
  end
  true
end
```

Learn #3: Abstraction. The second of those purposes is malleability. If I needed to create a new method, #previous_prime?, that also needed to check for primeness, I don’t have to rewrite any code at all, since I can still access my helper method. I can do this as many times as necessary for as many new methods that this information benefits. Enter the Single Responsibility Principle. A method should do one thing. This ties in well with the concept of abstraction. I’ve heard/read the term “DRY- Don’t Repeat Yourself”. Abstraction means we make that code less specific so it can be used easily for future tasks and I avoid repeating large chunks of code with small variation. 

Learn #4: Make your own data structures. This was a big ‘aha’ moment for me. When demonstrating solutions for ‘Hashketball’, our instructor quickly extracted the bulk of the large hash we had been operating on, and created a new hash where the character’s name pointed towards a value of it’s respective hash. This made accessing specific data by name much easier, and reduced the code required by subsequent methods.	

In the past week I have made use of all the above principles in my own coding labs and assignments and been amazed at the improvements in structure that my code has undergone. I've tried to be aware of these things and talk about them early and often when pairing so that we, as a team, can structure things abstractly from the beginning when it's clear how to implement, or else know that we may want to revisit something after we get the code working. 

My final thought to wrap up this post: repitition serves a funny purpose in programming. As we learn, it is essential. We write out new syntax many times to practice and learn most effectively. Yet, when programming, it is the enemy and must be avoided. Here's to many more 'aha' moments and learns in the weeks to come.




