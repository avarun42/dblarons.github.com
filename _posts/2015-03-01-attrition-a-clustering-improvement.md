---
layout: post
title: "Attrition - A Clustering Improvement"
comments: True
---

## The Problem

My current strategy for clustering in my game [attrition](http://www.aaronhsmith.com/2015/02/04/attrition-an-introduction/) considers only a few parameters: computer health (maximize), computer soldiers (maximize), player health (minimize), player soldiers (minimize), and number of rounds (minimize). By clustering the data, I hoped to pick a strategy for each round of the game that would maximize the likelihood of a computer victory.

This evening, after using Mahout's KMeansDriver to cluster this data, I fully realized what I have suspected all along -- these parameters alone do not give me a detailed enough picture of the game at any given point.

Consider two games. Both result in the same outcome; the computer wins with 50 health and 500 soldiers and the player loses with 0 health and 300 soldiers. However, in one game, the computer plays offensively against a defensive player. In the other game, the computer plays defensively against an offensive player. With my current clustering approach, the computer AI may choose to play with an offensive strategy against an offensive player -- likely resulting in defeat unless the computer has a sizable advantage in soldier numbers.

## A Possible Solution

To remedy this situation, I am considering a new approach. Instead of only clustering game outcomes, I should also consider the player and computer moves at each turn. I consider this "looking at the past". On the classification side, a round that results in a negative change in health of 3 should be preferred to a round that results in a change of 50, even if the final results of the games that those rounds occurred in were equivalent. I consider this to be "looking in the (hypothetical, but plausible) future."

For this strategy to work, the clustering and classification steps should roughly be as follows:

### Clustering

Cluster games based on health and number of soldiers for the player and computer at the end of the game, number of rounds, and the computer and player move for each round. This should stratify the clusters into strategies that are distinct beyond simply reflecting the outcome of a particular game. These clusters will ideally reflect the best strategy to combat a unique human player.

### Classification

Classify games based on the same characteristics that were used for clustering. When selecting a strategy, look for the strategy closest to the cluster that minimizes health/soldier loss for the computer and maximizes the loss for the player. I still need to do some research to decide if the second part is possible.

## Conclusion

With this new strategy for clustering my gameplay data, I hope to draw better conclusions for the computer to use during a game. The idea of using previous moves was already being considered, but selecting a strategy based on the soldier/health delta is new. Hopefully it will provide another layer with which I can differentiate good from bad strategies.
