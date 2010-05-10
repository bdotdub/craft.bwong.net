---
layout: post
title: Javascript call() function
post_date: May 9th, 2010
post_slug: javascript_call_function
---

I have recently been writing and playing with Javascript. I have had a decent
understanding of it for a while now, but recently I've been playing around with
some [node.js](http://nodejs.org/) apps and have really been able to explore the
language a bit more.

One thing [I recently discovered](http://github.com/visionmedia/express/commit/624cf93e2e1e5c079fc4da9ab56a23f98ac045ce#diff-3)
is the `call()` function. `call` is a function that you can call on
another function where you can pass in `this` as an argument.

Confused? Well, let's take it back a few steps. If you're familiar with
Object-oriented programming, the `this` variable is basically a reference to
itself. Let's take a look at some code:

{% highlight javascript %}
var Car = function() {
  this.wheels = 4;
  this.numberOfWheels = function() {
    alert(this.wheels);
  }
}

var vehicle = new Car();
// This will alert with the value 4
vehicle.numberOfWheels();
{% endhighlight %}

Now, what the `call()` function allows you to do is to pass in what `this`
is. Being able to do this allows you to some really cool stuff. Here is a
basic example:

{% highlight javascript %}
var alertMyFullName = function() {
  alert(this.fullName());
}

var car = {
  fullName: function() { return "BMW"; }
};
var person = {
  firstName: "Benny",
  lastName: "Wong",
  fullName: function() {
    return this.firstName + " " + this.lastName
  }
};

alertMyFullName.call(car);      // Will alert "BMW"
alertMyFullName.call(person);   // Will alert "Benny Wong"
{% endhighlight %}

This basically gives you the ability to call the function as if it was
a function of that object. Another cool use of this is the ability to
chain constructors. The following example is adapted from 
[Mozillas documentation of `call()`](https://developer.mozilla.org/En/Core_JavaScript_1.5_Reference/Objects/Function/Call):

{% highlight javascript %}
function Car(name) {
  this.name = name;
  this.wheels = 4;
}

function BMW() {
  this.country = "Germany";
  Car.call(this, "BMW");
}

var car = new Car("Aston Martin");
var bmw = new BMW();

alert(car.name);    // Will alert 
alert(bmw.name);    // Will alert "BMW"
alert(bmw.wheels);  // Will alert 4
{% endhighlight %}

This is hardly new to Javascript (since Javascript 1.3, released Oct 1998!),
but thought I'd share something I learned `:)`