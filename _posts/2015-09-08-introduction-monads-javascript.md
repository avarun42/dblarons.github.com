---
layout: post
title: "Week 3 - Introduction to Monads in JavaScript"
comments: False
---

## Topics

- `Maybe` monad

- Monad laws
  
  - Left Identity
  
  - Right Identity
    
  - Associativity

- Resources
    
  - [https://curiosity-driven.org/monads-in-javascript](https://curiosity-driven.org/monads-in-javascript)
     
  - [http://sean.voisen.org/blog/2013/10/intro-monads-maybe/](http://sean.voisen.org/blog/2013/10/intro-monads-maybe/)

## Introduction

This post is the third in a 12 week series of sessions that I wrote for and
teach at VandyApps, the programming club at Vanderbilt University. The sessions
explore functional programming topics in JavaScript and Haskell. If you like
continuity and want to start from the beginning, the first post can be found
[here](/2015/08/21/introduction-functional-programming-javascript/).

This week, we will use JavaScript to explore the `Maybe` monad. We will first
present a common problem that requires extraneous amounts of boilerplate code:
null checking. We will then learn the properties of a monad and see how
creating a maybe monad can isolate the boilerplate code required for null
checks.

-------------------------------

## Maybe Monad

Imagine that we work at a fictional web company. Our company has decided to
consume Elgoog's Maps API. We will provide the API with a city and it will
return to us the coordinates of that city's capitol. Elgoog returns a JSON
object as their response. Here is a successful response to a query:

```javascript
var response = {
    'location': {
        'country': 'USA',
        'city': {
            'name': 'Boston'
            'coordinates': {
                'latitude': 1234,
                'longitude': 2345
            }
        }
    }
}
```

Sometimes, Elgoog doesn't give us all the information we need. For instance,
they may not have the coordinates for our city, or the city we searched for may
not exist at all. Here are two examples of responses that indicate failure.

```javascript
var badResponse = {
    'location': null
}
```

```javascript
var badResponse = {
    'location': {
        'country': 'USA',
        'city': {
            'name': 'Boston',
            'coordinates': null
        }
    }
}
```

Now that we know what form responses can take, let's write code to retrieve
these coordinates from the response. We want to return an array containing the
latitude and longitude pair.

```javascript
function getCoordinates(response) {
    var latitude = response.location.city.coordinates.latitude;
    var longitude = response.location.city.coordinates.longitude;
    return [latitude, longitude];
}
```

What happens when we call this code with the successful response?

```javascript
var coords = getCoordinates(response); // => [1234, 2345]
```

We're done! Not quite; we still haven't tested the failed lookup response.  Let's
try that.

```javascript
var coords = getCoordinates(badResponse);
// => Uncaught TypeError: Cannot read property 'latitude' of null
```

Looks like that won't work. JavaScript gave us a TypeError (Java would throw
a NullPointerException). Do you see why the error is occurring? We are trying
to access the `latitude` property of `null`. Of course, `null` doesn't have
a `latitude` property, so JavaScript complains.

To solve this problem, we need to perform checks in our code to test for
`null` values before we access their properties. For better or worse, we can
put all of these null checks into a single `if`-statement by using
short-circuit evaluation.

```javascript
function getCoordinates(response) {
    var latitude,
        longitude;

    if (response !== null && response.location !== null 
        && response.location.city !== null 
        && response.location.city.coordinates != null) {
        latitude = response.location.city.coordinates.latitude;
        longitude = response.location.city.coordinates.longitude;

        if (latitude === null || longitude === null) {
            throw "Error: Coordinates cannot be null";
        }
    } else {
        throw "Error: Response object did not contain coordinates.";
    }

    return [latitude, longitude];
}
```

Phew, that was a lot of null checks. Unfortunately, they are necessary if we
are to be 100% sure that a TypeError exception won't be thrown unexpectedly.

But... `<hint>` maybe `</hint>` we can be smarter about how we perform null
checks.

## Introducing the Maybe Monad

In Haskell, the `Maybe` type is defined as follows:

```haskell
data Maybe t = Just t | Nothing
```

That declaration says that `Maybe` consists of `Just` something (`t`), or
`Nothing` at all.

Let's write our `Maybe` function in JavaScript and see how it can help us solve
the null check problem. Then we'll return to it and see how the magic works.

```javascript
Maybe = function(value) {
  var Nothing = {};

  var Just = function(value) { 
    return function() {
      return value; 
    };
  };

  if (typeof value === 'undefined' || value === null)
    return Nothing;

  return Just(value);
};
```

Let's try passing a few values to `Maybe`.

```javascript
Maybe(null) == Nothing; // => true
typeof Maybe(null); // => 'object'

Maybe('foo') == Nothing; // => false
Maybe('foo')(); // => 'foo'
typeof Maybe('foo'); // => 'function'
```

Now we can rewrite our function using `Maybe`.

```javascript
function getCoordinates(response) {
    if (Maybe(response) !== Nothing 
        && Maybe(response.location) !== Nothing && 
        Maybe(response.location.city) !== Nothing 
        && Maybe(response.location.city.coordinates) != Nothing) {
        var latitude = Maybe(response.location.city.coordinates.latitude);
        var longitude = Maybe(response.location.city.coordinates.longitude);

        if (latitude === Nothing || longitude === Nothing) {
            return "Error: Coordinates cannot be null";
        }
        return [latitude, longitude];
    } else {
        return "Error: Response object did not contain coordinates.";
    }
}
```

Well, that actually didn't help us very much. In fact, we had to _increase_ the
amount of code we wrote to use the `Maybe` monad. That sucks.

As it turns out, our current `Maybe` function isn't actually a monad. Why? Because it
doesn't conform to the monad laws.

## Monad Laws

There are three 'laws' that must be followed in order to create a monad.  These
laws are like the math version of an interface. If our monads conform to these
laws, then functions that act on the monad "interface" can be used on our
monads. If we add a few more laws (which will be covered later on in our
series) our monads can be used (stacked) with other monads.

Here are the laws in JavaScript. We will use `Maybe` for concreteness in these
laws, noting that in a formal definition of the laws, more general notation is
used.

Law 1: Left identity

```javascript
Maybe(x).bind(fn) == Maybe(fn(x)); // for all x, fn
```

Law 2: Right identity

```javascript
Maybe(x).bind(function(x){return x;}) == Maybe(x); // for all x
```

Law 3: Associativity

```javascript
Maybe(x).bind(fn).bind(gn) == Maybe(x).bind(function(x) {
  return gn(fn(x));
}); // for all x, fn, gn
```


## Bind

By now, maybe you've noticed that those laws use a 'bind' function that we
haven't written yet. As it turns out, the bind function is the last piece in
the monadic puzzle. Let's write one.

As described in the monad laws, bind needs to perform an action on the value
contained in our `Maybe`, then wrap the return value back up in another `Maybe`.

Let's take a look at the Haskell type signature for `bind`.

```haskell
Maybe a ->     (a -> Maybe b)     -> Maybe b
-- ^param 1     ^----------^param 2  ^return value
```

This signature tells us that the bind function takes a `Maybe` (param 1) and
a function that transforms a value (param 2) and returns a `Maybe` that contains
the transformed value. A common analogy for this process is opening a box
(`Maybe a`) that contains a value (`a`), then transforming that value into
another value (`a` into `b`), and wrapping the new value back up into the box
(`Maybe b`).

Note: Because of JavaScript's weak type system, the type signature of our
`bind` function is actually `Maybe a -> (a -> b) -> Maybe b`.

Let's add the bind function to our `Maybe` monad.

```javascript
Maybe = function(value) {
  var Nothing = {
    bind: function(fn) { return this; }
  };

  var Just = function(value) { 
    return {
      bind: function(fn) { return Maybe(fn.call(this, value)); }
    };
  };

  if (typeof value === 'undefined' || value === null)
    return Nothing;

  return Just(value);
};
```

Now we can rewrite our Elgoog client so that it uses the bind function.

```javascript
function getCoordinates(response) {
    var coordinates = Maybe(response).bind(function(r) {
        return r.location;
    }).bind(function(r) {
        return r.city;
    }).bind(function(r) {
        return r.coordinates;
    });

    var lat = coordinates.bind(function(r) {return r.latitude});
    var lon = coordinates.bind(function(r) {return r.longitude});

    if (lat === Nothing || lon === Nothing) {
        throw "Error: Coordinates cannot be null";
    }
    return [lat, lon];
}
```

This code looks a lot cleaner. Our intent is more clear, and the code is
relatively "flat". But by adding a few more functions to our monad, we can
improve this code even more.

```javascript
Maybe = function(value) {
  var Nothing = {
    bind: function(fn) { 
      return this; 
    },
    isNothing: function() { 
      return true; 
    },
    val: function() { 
      throw new Error("cannot call val() nothing"); 
    },
    maybe: function(def, fn) {
      return def;
    }
  };

  var Just = function(value) { 
    return {
      bind: function(fn) { 
        return Maybe(fn.call(this, value));
      },
      isNothing: function() { 
        return false; 
      },
      val: function() { 
        return value;
      },
      maybe: function(def, fn) {
        return fn.call(this, value);
      }
    };
  };

  if (typeof value === 'undefined' || value === null)
    return Nothing;

  return Just(value);
};
```

Phew, that's a lot. Let's take a look at the new functions:

```haskell
-- Returns true if the Maybe is Nothing and false otherwise.
isNothing :: Maybe a -> Bool
```

```haskell
-- Returns the value inside of the Maybe monad.
val :: Maybe a -> a
```

```haskell
-- From the Haskell wiki: "The maybe function takes a default value,
-- a function, and a Maybe value. If the Maybe value is Nothing, 
-- the function returns the default value. Otherwise, it applies 
-- the function to the value inside the Just and returns the result."
maybe :: b -> (a -> b) -> Maybe a -> b
```

Let's rewrite our Elgoog client one final time using our helpful new functions.

```javascript
function getCoordinates(response) {
    return Maybe(response).bind(function(r) {
        return r.location;
    }).bind(function(r) {
        return r.city;
    }).bind(function(r) {
        return r.coordinates;
    }).maybe("Error: Coordinates cannot be null", function(r) {
        return [r.latitude, r.longitude];
    });
}
```

That's it, our monad takes care of the rest.

By isolating our boilerplate null-checking code within the `Maybe` monad, we have
avoided the increase in indentation level and difficult refactoring that comes
with null-checking. Yes, we had to write a lot of code to create the monad, but
that code only has to be written once. If we use a monad library, we never even
have to write it ourselves! `Maybe` saves us time, typing, and helps us refactor
more easily.

