---
layout: post
title:  "Understanding Hash Tables"
date:   2017-04-21 12:49:20 +0000
---


Hashes are a game changer. I remember my first exposure to Ruby hashes around this time last year. When you’re dealing with a world of strings and arrays, looping and iterating in the name of the game. “Reverse this string”, “Is this a palindrome?”, “What’s the smallest number in this collection?”- the solution involves looking at every object in the data structure at least one time. With hashes (key - value pairs in Ruby), in many ways, that pattern of looping is rendered obsolete. I don’t have to remember which array[i] contains my desired value, I can simply call the value by it’s key. 

One of the biggest misconceptions I had at the time is a little bit evident in the previous statement. That is, I believed that hashes were essentially arrays with named indexes. It kind of makes sense to think this way as a beginner, especially considering that most people learn by comparing new and foreign ideas to more familiar concepts. But a friend of mine picked up on this misconception and explained the terrible truth: hashes are not just arrays with named indexes. 

![](https://media2.giphy.com/media/ZHkVpDiI3vIiY/giphy.gif)

No, hashes are, in fact, a data structure built around the mathematical transformation of a provided string (the key) into a pointer to the provided value. This transformation is called the hash function, and understanding how it works is key to understanding the larger structure. I can think of no better explanation than [this](http://blog.jgc.org/2013/04/a-non-mathematical-explanation-of-one.html) non-mathematical one that a classmate shared with me. The hashing itself is a one way transformation of the provided key to something new. I’m going to refer to that something new as a hashed_key, to avoid confusion with the provided value of the key-value pair. 

So say you are looking to create a hash table for some data.You want to provide a key of a restaurant name and retrieve a provided value consisting of a 1-5 score of the quality of that establishment. The first thing you do is create a array consisting of x number of spaces, which we refer to as buckets. For this example let’s say we’ll create 10 buckets. We will then write a function like this one:

![](https://i.imgur.com/Hy9RPWS.png)

What’s happening here is, we take all the letters in the string, convert them to their ASCII values, adds all those values together and then performs the modulo 10 operation on that sum. This is our mathematical transformation- from a string to a digit between 1 & 10. We use the the fact that the above provided key will always hash to the same integer (8 in our example) to our advantage. By choosing to store our value in the 8th bucket, we will now know where to look when retrieving the value at a later point in time. Because the key will always return 8, we know our value has to live in that respective bucket.

A logical follow up would be- what about when two keys hash to the same number? This called a collision and they can be accounted for a few ways. First- you’d probably use a larger number of buckets and therefore adjust your hashing function accordingly. If we have 50 buckets instead of 10, two strings are much less likely to hash to the same number when we amend line 4 to read str_total%50.

The second solution to create a linked list at each bucket, so if multiple sets of data needs to be stored in the same bucket, one can simply iterate through until finding one’s expected results, or concluding that the data doesn’t exist in our hash table.

That last part is the real beauty of hash tables. When searching for data in an array, the worst case scenario is that the data doesn’t exist in the array and we look through every item before concluding its absence. In a hash, we spend so much less time looking because the data can only ever exist in one bucket. We refer to hash tables as have constant time lookup for this reason, when perfectly implemented.

So back to my misconception- hashes aren’t simply arrays with named indexes because in that case we’d be no better off than numbered indexes. It would still require iteration & comparison to find the desired value or conclude it doesn’t exist in the collection. With hashes, we eliminate that need to iterate because we will always know exactly where to look for our data through the use of the associated hash function. 

Until next time!

