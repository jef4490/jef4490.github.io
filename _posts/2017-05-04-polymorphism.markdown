---
layout: post
title:  "Polymorphism"
date:   2017-05-04 12:57:16 -0400
---


Polymorphism is a term I’ve heard quite a bit in the last few weeks especially. It’s a core concept of Object Oriented Programming and something that gets thrown around a lot but what does it actually mean and what does it mean for us as developers? Let’s take a look!

![](https://technofriends.files.wordpress.com/2008/02/polymorphism.gif)

Polymorphism is the concept of sending the same message to different objects and get different results. In Ruby, for example, 
this equates to calling the same method on a a variety of objects and expecting the objects to behave differently. This is a keystone of the object orientated paradigm and when you think about it, the reasons become pretty clear. Namely, when we create applications we cannot predict the selections a user might make, and our objects need to being to handle the methods they might be called with dynamically.

<pun>Polymorphism takes on several different forms</pun>. One of the most common ways of implementing polymorphism is through the use of inheritance. With inheritance, a child object has access to the properties and methods defined in its parent class. Here’s a brief example of inheritance illustrating polymorphism.

``` 
class MagicalClothes
  attr_reader :name

  def initialize(name)
    @name = name
  end

  def say_name
    "My name is #{name}"
  end
end

class Pants < MagicalClothes
  attr_accessor :material

  def introduce
    "#{say_name} and I am made of #{material}. I am best described as #{self.class.to_s}!"
  end

end

class Shirt < MagicalClothes
  attr_accessor :material

  def introduce
    "#{say_name} and I am made of #{material}. I am best described as a #{self.class.to_s}!"
  end

end

peter = Shirt.new("Peter")
peter.material = "denim"
puts peter.introduce
  #=> My name is Peter and I am made of denim. I am best described as a Shirt!

david = Pants.new("David")
david.material = "cotton"
puts david.introduce
  #=> My name is David and I am made of cotton. I am best described as Pants!

```

In the above example, two classes draw upon the properties of a common parent, but express their differences in clothing type & individual material when calling their introduce methods. It is important to consider that I do not need to change the way I call the Pants vs the way I change the Shirt to receive the dynamically appropriate result I am looking for. If I had a collection of shirts and pants for example…

```clothing = [peter, david, diana, jeffrey]```

I don’t need to figure out if an individual member of that collection is a Shirt before operating on it. I can simply iterate over the collection like so.

```clothing.each{|item| puts item.introduce}```

There’s no need for conditionals to determine how to handle one item vs the next. There are other ways to implement polymorphism in Ruby also. A common one is [Duck Typing](http://rubylearning.com/satishtalim/duck_typing.html). Another is the use of a [decorator pattern](http://nithinbekal.com/posts/ruby-decorators/). I’ll touch on these in future posts.

Thanks for reading!
