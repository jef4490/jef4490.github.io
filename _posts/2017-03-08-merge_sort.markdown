---
layout: post
title:  "Merge Sort"
date:   2017-03-08 02:47:30 +0000
---


This past Saturday we were given some exposure to Algorithms and other traditional CS concepts. Based on conversations with programmers I know, it seems like the majority of what we’ve been working on during this course so far has been focused on preparing us for the average day as a Web Developer. Algorithms represent a slight departure from that, the subject is to music theory as our core curriculum is to performing. Study of algorithms informs the decisions we make when coding, but it’s most relatable when one has a base level of chops to work with and fluid understanding of convention and standards.

I went into to Saturday eager and excited and found it… kind of cool. That said, I was forced to confront some old friends that I hadn’t seen in an actual decade.

![](https://i.imgflip.com/1kycaq.jpg)

Graphing functions; that takes me back to 11th grade. Still, I found myself remembering it faster than I would’ve anticipated. Not logarithms though. I had to have that one explained to me in some pretty basic terms. And although I made a fool of myself algebraically in front of the Google engineer who was our instructor, I really enjoyed thinking mathematically again.

So what did we cover? Linear search vs. binary search and the concepts of Big-O, Big-Ω, and Big-θ notation. That was a lot to wrap my head around for a Saturday afternoon. Maybe I’ll talk about those in detail in a future blog post, but this one is going to be about a specific algorithm I had previous exposure to, now revisited through the lens of the Algorithms lecture I attended. 

Merge Sort-

Merge sort is, like many Algorithms, a classic solution to a classic problem. The problem is, how to sort of a collection of items. And I’ll interject to mention that the biggest takeaway I had on Saturday was hearing our instructor talk about why this matters, even in terms of our applications. We talked about how searching your database for information is directly responsible for the load times of your web applications, which can directly impact a user’s decision to stay or leave your app. A sorted database (through indexing, or other means) facilitates better searching algorithms and directly impacts the user experience. Sorting is important.

Back to merge sort. This is a way of sorting data that involves dividing your collection down repeatedly until you’re left with each element on it’s own. Then, you merge these elements into a larger array, comparing the values of the items being merged and making a switch when necessary. It’s a little easier to understand visually, in my opinion.

![](https://upload.wikimedia.org/wikipedia/commons/c/cc/Merge-sort-example-300px.gif)

Again, your array of n elements is broken down into n arrays of 1 element each. Then, you compare items two at a time (typically referred to as ‘left’ and ‘right’), merging them into a larger array of 2 items, ordered so that the elements are ascending. We then repeat the process, merging the two arrays of 2 elements into an array of 4 elements, comparing the left-most element of the left array with that of the right array, and pushing it into our new array. Then, we again compare the left-most elements of each sub-array, continuing to choose the smallest element first. This process of merging continues until we are left with one array of n elements, who are now sorted.

There are a couple ways to go about implementing this method, the two main approaches contrast techniques of iteration vs. recursion. Iteration is the more difficult to implement and I found significantly less elegant, so I want to focus on the more popular recursive method. 

```
def mergesort(list)
  #Divide
  return list if list.size == 1
  mid   = list.size / 2
  left  = list[0, mid]
  right = list[mid, list.size]
  merge(mergesort(left), mergesort(right))
end

#Conquer
def merge(left, right)
  sorted = []
  until left.empty? || right.empty?
    if left.first <= right.first
      sorted << left.shift
    else
      sorted << right.shift
    end
  end
  sorted.concat(left).concat(right)
end
```

Here the strategy is really separated into the tasks of “divide” & “conquer”. Divide is where we employ recursion. Like in all recursion, we need to set a base case to avoid creating an infinite loop. We can definitively say that an array of 1 element is both indivisible and fully sorted, so we make that our base case with a conditional. 

The code will begin to return values when the subarrays reach length of one, and then will undergo the sorting through merging mechanism. We can see the comparison of the left and right subarrays, ultimately returning a larger array to undergo the same process one level higher.

Note that this code accounts for an array that cannot be evenly divided all the way to the base case (not a power of two) during the conquer portion. 

So why merge sort? It’s quite efficient. In terms of machine operations, it’s worst case is best summarized as O(n log n) which compares favorably to other sorting algorithms. It does take more auxiliary space to perform the sort, however, which has trade offs. All in all, it’s cool to be able to look at these under-the-hood solutions built into various parts of the languages we use to code. Here’s to more algorithms.



Source:

https://en.wikibooks.org/wiki/Algorithm_Implementation/Sorting/Merge_sort#Ruby
http://www.ee.ryerson.ca/~courses/coe428/sorting/mergesort.html

