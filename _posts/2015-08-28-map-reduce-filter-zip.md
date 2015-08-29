---
layout: post
title: "Week 2: Map / Reduce / Filter / Zip in JavaScript"
comments: False
---

## Topics

- Map

- Reduce/Fold

- Filter

- Zip

## Introduction

This post is the second in a 12 week series of sessions that I wrote for and
teach at VandyApps, the programming club at Vanderbilt University. The sessions
explore functional programming topics in JavaScript and Haskell. If you like
continuity and want to start from the beginning, the first post can be found
[here](/2015/08/21/introduction-functional-programming-javascript/).

This week, we will cover some of the seminal functions in functional
programming: Map, reduce, filter, and zip. These functions will largely
replace our need for loops when writing functional code.

As a running example throughout this session, we will consider a web
application that tracks users, their relationship statuses, and their incomes.

-------------------------------

## Map

I just queried my database to find users who are about to get married. How
do I update every user in this array so that their marriage status
is `true`?

```javascript
var users = [
    {
        'username': 'johnstack',
        'married': false
    },
    {
        'username': 'susyqueue',
        'married': false
    },
    {
        'username': 'marytrie',
        'married': false
    },
    {
        'username': 'davidheap',
        'married': false
    }
]
```

How would we solve this problem using our current knowledge? Let's try
a for-loop:

```javascript
function updateUsers(users) {
    for (int i = 0; i < users.length; i++) {
        users[i].married = true;
    }
}
```

There are a few problems with this code: 

1. It's **not readable**. Even though there's only one for-loop, only
   a programmer would understand this "jargon." Even experienced programmers
   can find deciphering a for-loop, especially when nested, to be tricky.
   
2. Second, adding more loops would cause our code to use **more indentation
   levels**. While largely a syntactic detail, it can be a nuisance to read
   heavily indented code, and we are in the business of writing code not for
   the computer, but for the reader.

3. Third, the loop **modifies our original array**. Not doing so would require even
   more code.

Can we do better? Once again, we can. The map operator allows us to simplify
this operation.

```javascript
function updateUsers(users) {
    // This is read as: "For each user, set married to true."
    users.map(function(user) {
        user.married = true;
        return user;
    });
}
```

In the above code, the map operation applies the anonymous function passed
as its first argument to every element of the users array. The map function
then returns a new array with the resulting set of data; the original `users`
array is unmodified.

What if we wanted to build a new array consisting of only the newlyweds
names?

```javascript
function getNewlyWeds(users) {
    var usernames = users.map(function(user) {
        return user.username;
    });

    console.log(usernames); // => ['johnstack','susyqueue', 
                            //     'marytrie', 'davidheap']
}
```

## Filter

I just queried my database again for all of the women that use my product. I'm
about to run an advertising campaign for wedding dresses, so I only want to
target unmarried women.

How can I `<hint>` filter `</hint>` my dataset so that it only contains
unmarried women.

```javascript
var femaleUsers = [
    {
        'username': 'perlgirl',
        'married': true
    },
    {
        'username': 'susyqueue',
        'married': false
    },
    {
        'username': 'marytrie',
        'married': false
    },
    {
        'username': 'annboolean',
        'married': true
    }
]
```

How do we solve this problem using our current knowledge? Surprise, surprise...
use a for-loop.

```javascript
function filterForUnmarried(users) {
    var unmarried = [];
    for (int i = 0; i < users.length; i++) {
        if (users[i].married === false) {
            unmarried.push(users[i]);
        }
    }
}
```

This for-loop isn't very readable. If another developer reads this later,
they will have to do quite a bit of thinking to discover your original intent.

Can we do better? Of course we can! Let's use a filter.

```javascript
function filterForUnmarried(users) {
    return users.filter(function(user) {
        return user.married === false;
    });
}
```

The function passed to filter should always return a boolean value. Each
object in the list will be passed to the function and the resulting boolean
value will determine whether or not the object should be included in the
resulting list. Note that, like map, filter does not modify the original
users array.

What if we want to lift our user married anonymous function into the global
context (i.e. lambda lifting like we saw last week)?

```javascript
function isMarried(user) {
    return user.married === true;
}

function filterForUnmarried(users) {
    return users.filter(!isMarried);
}
```

Is this final example more readable than our for-loop? Yes! In fact, our
intent is readable as a cohesive sentence: "filter the users array for users that
aren't married."

## Reduce

My next task is to calculate the total income of everyone living in a Towers
suite. There are four women, so I queried for their usernames and income.
How can I best sum up the income for these four women?

```javascript
var femaleUsers = [
    {
        'username': 'perlgirl',
        'income': 1000
    },
    {
        'username': 'susyqueue',
        'income': 2000
    },
    {
        'username': 'marytrie',
        'income': 500
    },
    {
        'username': 'annboolean',
        'income': 200000000
    }
]
```

First, let's try using a for-loop.

```javascript
function sumIncome(users) {
    var total = 0;
    for (int i = 0; i < users.length; i++) {
        total += users[i].income
    }
    return total;
}
```

That's fairly readable, but I think we can do better. To improve this function,
we will use a `reduce` method. This method takes an array and reduces it into
a single value.

```javascript
function sumIncome(users) {
    // Each element of the users array is selected and 
    // collapsed into the `initialValue` of 0.
    return users.reduce(function(prev, curr, index, array) {
        return prev.income + curr.income;
    }, 0);
}
```

Reduce tends to more "complicated" to newcomers than either map or filter, but
it's surprising how often it gets used. At least in this example, it may not be
obvious where the improvement comes from. But when we start combining higher
level functions to transform our data, the utility will be more obvious.

Okay, so that's sort of an improvement... But let's make things a little more
fun, shall we?

## Composing Higher Level Functions

What if I have an array of users, some married and some unmarried, all with
incomes, and I wish to sum up the incomes of only the married people.

```javascript
var allUsers = [
    {
        'username': 'perlgirl',
        'married': true,
        'income': 1000
    },
    {
        'username': 'johnstack',
        'married': false,
        'income': 450
    },
    {
        'username': 'susyqueue',
        'married': true,
        'income': 300
    },
    {
        'username': 'marytrie',
        'married': true,
        'income': 760
    },
    {
        'username': 'davidheap',
        'married': true,
        'income': 980
    }
    {
        'username': 'annboolean',
        'married': false,
        'income': 200000000
    }
]
```

If we just use for-loops, we end up nesting our code several levels deep and it
quickly becomes unreadable.

```javascript
function sumMarriedIncome(users) {
    var total = 0;
    for (int i = 0; i < users.length; i++) {
        if (users[i].married) {
            total += users[i].income;
        }
    }
}
```

How can we improve this? How about `filtering` for married individuals, then
`reducing` their income? We can reuse our isMarried function from before.

```javascript
function sumMarriedIncome(users) {
    return users.filter(isMarried)
                .reduce(function(prev, curr, index, array) {
                    return prev.income + curr.income;
                }, 0)
}
```



If we write a sum function for user incomes, we can make the code even more
readable!

```javascript
function sumIncome(user1, user2) {
    return user1.income + user2.income;
}

function sumMarriedIncome(users) {
    return users.filter(isMarried).reduce(sumIncome);
}
```

This function reads almost like English: "Filter for users that are married
and then sum their income."

Instead of a bunch of [, +, and = signs, we now have a readable piece of
code. We don't even need comments because this code is 100% self
documenting.

## Zip

A few of my users have decided to purchase dogs. I queried my database for the
users that decided to get dogs, and I queried the local shelter's database for
the first `n` (where `n` is the # of users that want dogs) dogs that need to be
adopted. How can I pair these users up with their new pets and print the new
pairings out?

```javascript
var owners = ['susyqueue', 'annboolean', 'johnstack', 'daveheap'];
var pets = ['sam', 'scout', 'old yeller', 'buddy'];
```

Let's try this with a for loop.

```javascript
function pairDogs(owners, pets) {
    if (owners.length !== dogs.length) {
        return;
    }

    var pairings = [];
    for (int i = 0; i < owners.length; i++) {
        var unit = [];
        unit.push(owners[i]);
        unit.push(dogs[i]);
        pairings.push(unit);
    }

    pairings.map(function(pair) {
        console.log("Owner: " + pair[0] + " Dog: " + pair[1]);
    });
}
```

Okay, so part of our function is functional, but we still have to pair the
dogs in the first place. If we wanted to pair cats later on, we would have
to write another pairing function. I'm too lazy to do that!

Well, unlike several other langauges, JavaScript unfortunately doesn't
provide this functionality in the standard library. But, as witty functional
programmers, we can add the functionality ourselves.

What we want is a `zip` function that takes two lists and "zips" them
together. Like the above function, it will require the lists to be the same
length.

```javascript
function zip(array1, array2) {
    if (array1.length !== array2.length) {
        return;
    }

    var pairings = [];
    for (int i = 0; i < array1.length; i++) {
        var unit = [];
        pairings.push(unit.push(array1[i]).push(array2[i]));
    }
    return pairings;
}
```

Now that we have a proper zip function, we can simplify our dog to owner
pairing function.

```javascript
function pairDogs(owners, pets) {
    zip(owners, pets).map(function(pair) {
        console.log("Owner: " + pair[0] + " Dog: " + pair[1]);
    });
}
```

Many people might point out that, yes, our `zip` function still uses for loops.
We probably could have written it so that we didn't need the for loop, but
I left it there to make a point: Sometimes it is helpful for us to isolate
ugly, but necessary, boilerplate code in a higher level function that we can
later use to write readable, maintainable code.

"Zips! Coming to a functional language near you!"

----------------------------------

Questions? Comments? The Hacker News comments are
[here](https://news.ycombinator.com/item?id=10138582).
