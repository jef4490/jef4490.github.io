---
layout: post
title:  "On Currying"
date:   2017-04-11 01:10:33 +0000
---

During the last week at Flatiron, we’ve been learning React, a Javascript library for building user interfaces. I’ve found it nothing short of game-changing when compared to the single page JavaScript-powered clients we made two short weeks ago. In fact, part of our work has been to rebuild our JavaScript applications using React. Gone is the obligation of manipulating the DOM, React does this under the hood and with a great deal of efficiency and intelligence. Instead, I get to focus more on describing what the data and presentation look like. I’ll likely write more about what makes React cool in future blog posts, but today I’m going to talk about a smaller topic, not specific to React (though commonly encountered), but related to JavaScript as a larger subject. 

![](https://media1.giphy.com/media/g8rEwOqIStrBC/giphy.gif)

![](https://media.tenor.co/images/f8ee93747713547c07f7ed2cef4a89bc/tenor.gif)

One thing I’ve stumbled upon during some of our course work is something like this:

```
onClick={this.props.onClick.bind(null, this.state.selectedIndex}
```

This is called currying and it caught me off guard because it looked a little familiar, but still very foreign. The familiar; a common practice in React (and Javascript/jQuery in general) is binding callbacks for event handlers to the context of the class in which they were created. For example:

```
this.onChange = this.onChange.bind(this)
```

I had used the above example enough in my programming to understand that what was happening: the creation of a new function that had access to the variable called ‘this’ as it was in the context of the object in which the function was defined. What I didn’t understand however, was that the use of the bind was not limited to merely the ‘this’ variable and that it is such an occasion taking place in the first example. That is, we can bind other variables into a function. So after some experimentation, I’ve come to understand the way .bind() works a little bit better. 

Take the following code:

![](http://i.imgur.com/8i30zup.png)

We have a class called Dish which is instantiated with a single argument, in the example ‘chicken’. This argument is then set to a class property of type during the constructor function. A class method, sayType, logs the instance’s type to the console. Pretty straightforward.

```
a.sayType()
// #=> chicken
```

It becomes a little more complex if we assign this function to a new variable, therein making it a callback.

```
var b = a.sayType

b()
// #=> Uncaught TypeError: Cannot read property ‘type’ of undefined
```
The above error is, of course, the primary inspiration for the use of .bind(). When we call sayType as a call back through the variable b, we have now lost the context of the object in which the method was originally defined. So inside our constructor we add a line like the one on line 4:

![](http://i.imgur.com/Kt1EarU.png)

As you can see, b() now returns the expected value of ‘chicken’. It’s also not too much of a stretch of the imagination to try add a second argument to our sayType() function. With the function still bound in constructor, the following makes sense.

![](http://i.imgur.com/E8HJIwA.png)

Both a.sayType() and b() are functioning properly, and the supplied argument is used in the console.log statement producing the results we expect. But if we start to change the bind() function we’ll see some unexpected results in our code.

![](http://i.imgur.com/h2WDyr4.png)

The first argument of bind will always be applied to the value of ‘this’ inside the callback. So the reason we get the unexpected ‘undefined tikka masala’ on line 19, is the fact that this.type is an undefined value. When we alter the code to pass merely ‘this’ into the console, it behaves as expected.

![](http://i.imgur.com/s8gpz8P.png)

So the above code deals with the first argument of the the bind() function, but the power of the function lies in the ability the pass in additional arguments too. 

![](http://i.imgur.com/undefined.png)

Here, we bound not only the context, but also the argument that variable, ‘curry’, stands in for. See that on lines 15 and 20, we call the method with no supplied arguments. This is what currying is all about, partial application of a function’s arguments. It’s not too hard to imagine a scenario where we have data that is needed for the function to properly accomplish it’s task, but it may not be convenient to pass it in at the time the function is called. Currying allows us to essentially prepare the arguments in advance. Here’s another example in a slightly different context:


![](http://i.imgur.com/lgwdZhP.png)

Notice line 12. We use the null keyword to maintain the previous context binding on line 4, while adding the second argument of ‘tikka masala’. Notice, how the bound argument fills the first of two arguments that reviewDish() accepts. Then the only supplied argument during the method call on line 19 is applies to the curry variable.

![](http://i.imgur.com/egSknq2.png)

Unfortunately we run into the same issue as we did before, where handleComments doesn’t have access to ‘this’ when passed as a callback. In our final example, we wire everything up correctly and can now review our meal later.

![](https://i.imgur.com/7o1WUyM.png)

One can imagine the practical applications of these tiered approaches to providing arguments. There are situations where it makes more sense to hold onto pertinent information within the context of the method’s class than to have to pass along with the callback. Event handlers are quite common in a React context, so it makes sense to make your code as modular as possible and if currying allows us to avoid passing around an extra variable, our code is stronger for it. 

My hope is that this helped shed some light on what’s going on underneath the hood of .bind(). I know, for me, it was really valuable to see exactly which permutations of the code would result in errors for both callbacks and direct calls and my understanding is the better for it. 

May all of your currying problems be NaN/naan/non-issues.

![](http://cdn.smosh.com/sites/default/files/2015/12/dad-jokes-star-trek.gif)

Until next time!

