---
layout: post
title: "Attrition - New Plan, Same as the Old Plan"
comments: True
---

I've got a new plan; it's the same as the [old plan](https://www.youtube.com/watch?v=g_SWP3qI7Rg). But it's going to work.

First, a disclaimer: I'm not working on this project because I want to make a fun game -- or a smart AI for that matter. I'm working on this project because I want to learn about machine learning. If I wanted a half-decent AI for this game, I would have written the recommendation/nearest neighbor system I described in my last post. But that would have been simple.

Today I want to take the project back to basics (and by that, I mean as complicated as possible; for learning purposes, right?). I'm reintroducing classification into the equation, but this time I (sort of, but not really) know what I'm doing.

1. Instead of clustering all of the data collected, I'm only going to cluster data on the human opponent's moves. Why? Because making calculations based on raw health and unit numbers is easy, but making decisions based on the opponent's strategy is tough (if not impossible).

2. I am going to use 3-5 clusters. Too many clusters will make classification unwieldy. These clusters will hopefully represent different strategies that a human player exhibits.

3. Classification will use data that is labelled as belonging to one of the 3-5 clusters for training. Once trained, the algorithm's accuracy can be tested on a small amount of different labelled data.

4. During a game, the machine can classify its opponent as belonging to one of these clusters. The opponent may be reclassified into a different cluster as the game proceeds if its strategy changes.

5. The computer can use its guess of the player's strategy in order to formulate attacks that will do the optimal amount of damage to the player. For example, if the computer expects the player to deal an offensive blow with 90% of its soldiers, the computer can use 100% of its soldiers to defend, minimizing damage done.

It is unlikely that this will produce a very effective AI. However, I'm very interested to see if the computer can learn to distinguish human strategies (or even if real human players exhibit discernible strategies at all).
