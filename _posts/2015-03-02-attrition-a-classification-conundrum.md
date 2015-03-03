---
layout: post
title: "Attrition - A Classification Conundrum"
comments: True
---

After writing my post about the clustering problem I ran into last night, I set my computer and the project aside until this morning. Instead of diving into the code today, I read several chapters of [Mahout in Action](http://www.manning.com/owen/), dug through Mahout documentation, and (most importantly) kicked back to let myself think. The conclusion I have come to is that classification does not fit our application domain.

Originally, our vision for the [Attrition](http://www.aaronhsmith.com/2015/02/04/attrition-an-introduction/) project was that we would first cluster the simulated game data, resulting in a set of distinct groups into which we can then classify new games as they are played. While the clustering step still makes sense to me, the classification step does not. This is not a knock on classification, but rather my own (limited) understanding of it. I now suspect that what I previously understood to be classification is actually a nearest neighbor solution.

Consider a round during a game of Attrition. There is data on previous games that were won and it has been clustered. To choose a strategy for the computer in round N, plot the data available up to this point in the game (health, soldiers, player moves, etc.), ignoring any data that is not available yet (future rounds, total number of rounds). Then, select the cluster that is closest to this point that results in a win. Ideally, this would be the computer's best response to the strategy that the player has exhibited throughout the current game.

I see a few problems with this approach. First, if the current number of rounds exceeds the number of rounds in a simulated game, does the simulated game data become useless? Second, will the computer get "stuck" choosing the same strategy repeatedly simply because it chose that strategy in the past, and how responsive will the computer be to changes in player strategy? Third, what is the benefit of clustering the data at all? 

From my research today, my incorrect understanding of classification appears to be more similar to the "nearest neighbor" approach -- wherein a strategy is selected based on how close it is in n-dimensional space to another, winning, strategy. But what is the benefit of clustering data if a nearest neighbor approach might give me a strategy that is more representative of the current game climate anyways? The only reason that comes to mind is that clustering might in fact create an "average" winning game in response to a particular strategy that is more ideal that any one of the individual games that are represented in that cluster. If this turns out to be the case, clustering is a worthwhile improvement -- but I have my doubts.

While I am eager to continue implementing my ideas for the Attrition project, I am hesitant to continue until I have hammered out a clear plan for moving forward. I believe that I am closer to my goal after today's research, but I suspect that tomorrow I may again find myself discovering more of my own misunderstandings (my learning curve right now feels like a slow implementation of logistic regression - continuously approaching an ideal value, but at a decreasing rate). If I am completely off-base in my current approach, feel free to let me know in the comments.
