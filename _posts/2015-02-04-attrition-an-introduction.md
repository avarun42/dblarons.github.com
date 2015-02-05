---
layout: post
title: "Attrition - An Introduction"
comments: True
---

### The Game

This semester, I am working on a machine learning project, currently named [Attrition](https://github.com/bad-intel/attrition), with a few friends. Attrition is a simple game that will serve as a playground within which we can learn about machine learning. 

The rules of the game are as follows:

1. There is one human and one computer player.

2. Each player in the game starts with 100 health and 1000 soldiers.

3. Before each turn, each player chooses how many soldiers to attack with and how many to defend with. (defenders = 1 - attackers)

4. About 10% of attacking soldiers will perish. There is a 10% increase in soldier population based on the number of defenders a player has for that round.

5. Defending reduces the impact of an attack.

These rules, while simple, can easily be expanded upon. A few examples that we have in mind are (a) allowing multiple humans to play against one computer and (b) adding another troop category (e.g. tanks).

### Why Clojure?

We are working on this project in Clojure. We chose Clojure because its Java interop capability gives us access to machine learning libraries that are well maintained and documented. In addition, the language provides an interactive development environment where we can test various algorithms and their accuracy.

### Machine Learning

This week, we reached the point where we need to begin working with machine learning. We have been watching Andrew Ng's machine learning lectures ([available here](https://class.coursera.org/ml-005/lecture)) and are finishing up week four. The lectures have given us insight into how the algorithms we plan to use are implemented.

Our current plan for the machine learning aspect of the project was sparked during a dinner discussion I had with a Vanderbilt professor. It hinges on the idea that certain players will have unique strategies and that the computer can recognize and respond to those strategies in real time. The learning strategy can be broken down into two pieces.

1. Clustering (or, unsupervised learning): We will simulate thousands of games with a Stochastic Simulation. This simulation will hopefully provide an ample amount of data from which we can make deductions about effective strategies. We plan to use unsupervised learning to identify groups of games where the computer was successful. At first, we will only be measuring success by the amount of health left at the end of the game and whether or not the computer won. The clusters that we hope result from the simulations will represent winning strategies that the computer used against human players that shared a similar strategy. 

2. Classification (or, supervised learning): We designed our game to be played for 15-20 rounds in a single game. Throughout the game, the computer should become a "smarter" opponent. That is, the computer will choose a strategy from the clusters created in part 1 to match the human's current strategy. After each round, the computer will have more data with which to work and will again use classification to choose a strategy for the upcoming round. Later on, we hope to feed the data we collect from real games against humans back into our clusters to maximize the effectiveness of our AI.

We hope that classification and clustering together will give our AI the tools it needs to effectively combat human components. As mentioned above, we plan to expand the AI to be able to hand multiple human opponents synchronously.

### Last Words

You can check out the code for [Attrition on Github](https://github.com/bad-intel/attrition). Feel free to ask questions in the comments. 
