---
layout: post
title: "Week 1: Introduction to Functional Programming in JavaScript"
comments: True
---

## Topics

- Using a REPL.

- Welcome to JavaScript.

- Anonymous functions: Lambdas, closures, lambda lifting.

## Introduction

This post is the first in a 12 week series of sessions that I wrote for and
teach at VandyApps, the programming club at Vanderbilt University. Welcome to
the VandyApps Advanced Programming sessions. Let's begin.

------------------------------------

## Using a REPL

Think back to CS 251. Did you ever wish that testing out a function or
algorithm didn't require your code to compile cleanly? Sometimes it was
a total pain to compile, run, and use your program, just to find out that
a function or method didn't work as you expected it to. Some of you may have
even created a _new_ project just to test out that function in a clean
environment that would compile cleanly.

Well, fear no more. Progressive™ languages have a solution for this problem, and
it's called the REPL. REPL is an acronym: It stands for "Read, Evaluate,
Print, Loop," and that's exactly what it does. It allows you to write code,
import libraries, and more without compiling and running a full project.

If you've used the Python prompt in your terminal before, you've
already used a REPL. Similarly if you have opened up the Chrome or Firefox
developer tools.

Let's open up the REPL in your browser to give it a whirl.

```javascript
4 + 5 // => 9

console.log(4 + 5) // => 9

console.log("Learn. Design. Develop.") // => "Learn. Design. Develop."

// Hint: Use Shift-Enter to insert a newline in the Chrome REPL.
for (var i = 0; i < 10; i++) {
    console.log(i); // => 1 \n 2 \n ... \n 10
}

```

## Welcome to JavaScript

JavaScript is a prototypal, weakly and dynamically (duck) typed language.
Let's break each of those words down.

1. Prototypal – Most of the languages we work with in school are object
   oriented. Haskell, which we will look at later in the seminar, is purely
   functional. JavaScript is an oddball. It is prototypal, which means that
   new objects are created by cloning them from other objects. However,
   JavaScript is multi-paradigm, in that it can also be used in a functional
   or object oriented manner. We will focus on it's functional features in
   these seminars.

2. Weakly typed – Unlike Python and Ruby, JavaScript is weakly typed.
   Strongly typed languages will either not compile or throw errors when
   an incorrect type is passed to a function. JavaScript, on the other hand,
   is weakly typed, so type conversions happen implicitly. Sometimes this
   can cause unexpected behavior.

3. Dynamically typed – Unlike C++ and Java, when writing JavaScript you do
   not need to declare types in your code. The "var" keyword declares
   a variable and that variable can be reassigned to an object or primitive of
   a different type without throwing an error. This will save us some typing
   and makes it easy for us to prototype code quickly.

## Anonymous Functions

Functions in JavaScript look like this:

```javascript
function doSomething(param1, param2) {
    // ...
}
```

We can then call that function:

```javascript
doSomething(1, 2);
```

What if we want to write a function that doesn't exist at the top level (in
global scope in the case of JavaScript) – i.e. a local function? We can use
an anonymous function, also known as lambdas.

```javascript
function doSomething(param1, param2) {
    // We can set anonymous functions to variables.
    var myFunc = function() {
        var a = 1;
        var b = 2;
        return a + b;
    };

    // We can call that function in our code.
    myFunc(); // => 3
}
```

In the `doSomething` function, we assign an anonymous function (so-called
because it doesn't have a name when we define it) to the `myFunc` variable.
We can then call that function by invoking `myFunc` .

Anonymous functions give us the reusable property of normal functions, but
with limited scope.

```javascript
function doSomething(param1, param2) {
    // Our anonymous functions can take parameters.
    var paramLambda = function(a, b) {
        return a + b;
    };

    paramLambda(1, 2); // => 3
    paramLambda(2, 3); // => 5
    paramLabmda(3, 4); // => 7
}
```

In fact, because they can capture their environment, lambdas can sometimes give
us even more power than normal functions. When a lambda captures its
environment, we give it a special name: Closure.

## Closures

```javascript
function doSomething() {
    var envVar = "I'm part of the environment.";
    var paramVar = "I was passed as a parameter.";

    // Closure property – Anonymous functions can capture 
    // their environment.
    var closure = function(param) {
        console.log(envVar); // => "I'm part of the environment."
        console.log(param); // => "I was passed as a parameter."
    }

    closure(paramVar);
}
```

As you can see, our closure simplifies our anonymous function by capturing the
environment. That way, we don't have to pass so many parameters to our
function. For a lot of simple operations that need to be reused within one
method, closures are the best choice.

But what if we decide that we want to use that lambda within a different
function than the one in which it was defined? 

Here we have a `userLogin` function that was written as a lambda and an
example user object. The `userLogin` function contains a lambda,
`loginStateChange`, that flips the state bit for a user. 

```javascript
var johnDoe = {
    id: 1,
    loggedIn: false
};

function userLogin(user) { 
    // loginStateChange is a closure – It captures the user object.
    var loginStateChange = function() {
        user.loggedIn = !user.loggedIn;
    }

    console.log(user.id);
}
```

At some point, we decide that we want to write a `userLogout` function
because our users are complaining that we track them on the internet.
Unfortunately, our cookie system is tied to a user login session, and we
don't yet have a way to tell when to turn off our tracking cookies.

```javascript
function userLogout(user) {
    var loginStateChange = function() {
        user.loggedIn = !user.loggedIn;
    }

    console.log(user.id);
}
```

Hmmm... our `userLogout` function contains copy-pasted code from our
`userLogin` function. Being the lazy programmers that we are, we don't want
to maintain two sets of the same code. Fortunately, we're in luck. We can
perform a "Lambda Lift" in order to move the anonymous function into the
global scope.

## Lambda Lifting

Lambda lifting is when you "lift" a lambda out of the function where it
resides and make it a top-level definition.

When you lift a lambda, you must consider whether the lambda captures
its environment (is a closure). If it does, you must make a parameter for
every environment variable that is captured (or encapsulate all of them
into an object and pass that object) in the new top-level definition.

We'll lift the `loginStateChange` closure into a top-level definition that
we can reuse in both the `userLogin` and `userLogout` functions.

```javascript
// Lifted lambda.
function loginStateChange(user) {
    user.loggedIn = !user.loggedIn;
}

// Now, we can rewrite our two functions that make use of the lambda.
function userLogin(user) {
    loginStateChange(user);
    console.log(user.id);
}

function userLogout(user) {
    loginStateChange(user);
    console.log(user.id);
}
```

Notice how natural the transition from lambda/closure to top-level function
is. When you are developing software in a language that supports
anonymous functions, many of your functions will begin life as helper
functions that are specific to some top-level definition. But as
requirements grow and the operation encapsulated by the lambda requires reuse,
the lambda is lifted out of its environment and into the global scope.

In short, lambdas, closures, and lambda lifting are all about optimizing code
reuse.
