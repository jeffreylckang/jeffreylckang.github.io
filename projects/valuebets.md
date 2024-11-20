---
layout: page
title: Using ML to Identify Value Bets
permalink: /valuebets
---

# Identifying Value Bets in Sports Using Machine Learning

*How can I make money in sports betting?* If you're like me, you probably asked yourself this question after confidently placing what felt like a sure bet--only to end up emotionally damaged at the end of the game. Throughout my entire amateur sports betting career, I can definitively say that I'm net negative. While that isn't ideal, it's something I'm okay with because betting for me has always been about having fun. But recently I've started wondering: what if I actually tried to approach sports betting more seriously? Is there some strategy or method I could use to identify what I think could be considered "good" bets?

So I thought about sports betting and this project is a reflection of my thought process on how to potentially be net positive. But before I dive into the details, let me start off by saying that I'm not an expert in this domain. Furthermore, the findings of this project are not intended as advice or recommendations. This project is for my amusement and an opportunity to see if I can apply some of the critical thinking skills I developed during my PhD to a topic I'm passionate about. 

#### Here's a short abstract about the results and what I learned

Write abstract

Use this to skip to any section:
[Motivation](#motivation)
[Data](#data)
[Methodology](#methodology)
[Results](#results)
[Conclusion](#conclusion)

## Introduction

Now it's clear what my objective is: to earn a profit in sports betting. A quick Google search revealed 2 main strategies to achieve this outcome: **arbitrage betting** or **value betting**. Arbitrage betting involves placing multiple bets on the same event, often using different bookmakers, to guarantee a positive return. This works because bookmakers typically set different odds for the same bets. Value betting on the other hand is a strategy where bettors identify bets they believe have a higher probability of occuring than what the bookmaker believes when setting the odds. 

Value betting immediately seemed more intuitive to me--I often evaluate betting events such as whether Lebron James will score more than 15+ points by judging whether the odds of this event reflect how likely I think the event will occur. 

Before we go through an example, let me provide a quick refresher on odds. In this project, I'm going to be using the American odds format, which represents odds with + or - sign to indicate the direction of how likely that event is to occur. The plus sign indicates an unlikely event whereas the minus sign indicates a likely event. It may be more intuitive (at least to me) to think of this using probability, so the odds of -200 suggests a 67% chance of occuring. The formula to convert American odds to probability is:

For negative odds:
$$P = \frac{|\text{Odds}|}{|\text{Odds}| + 100}$$

For positive odds:
$$P = \frac{100}{\text{Odds} + 100}$$

Now for an odds of -200, if you place a $10 bet, you would receive $5 for a total payout of $15. 

Profit for negative odds:
\[
\text{Profit} = \frac{\text{Bet Amount}}{|\text{Odds}|} \times 100
\]

Profit for positive odds:
\[
\text{Profit} = \frac{\text{Bet Amount} \times \text{Odds}}{100}
\]

Okay back to my example. If the odds for Lebron James scoring more than 15+ points is listed as -200, but I believe the odds should be closer to -300, then I see this as a "good" bet. This is what is referred to as a value bet: a situation in which I believe the "true" odds are higher than -200, or the probability for this event to occur is higher than 67%. 

## Motivation

One of the challenges I quickly noticed with identifying value bets in sports is how subjective this process feels. Unlike a coin flip, where the true probability of landing heads is objectively 50% (or American odds of +100), there isn’t an easily obtainable “ground truth” for an event like whether LeBron James will score more than 15 points in a game. Despite this, maybe I could brainstorm a way to arrive closer to the "ground truth" without knowing exactly what it is. All I have to do is reliably identify value bets with more than just my opinion and then I could theoretically turn a profit. 

For starters, a simple and hypothetical method might be to survey a thousand sports fans and ask their opinion about the probability of an event occuring. This idea leverages the [wisdom-of-the-crowd effect](https://en.wikipedia.org/wiki/Wisdom_of_the_crowd) and is a pretty solid approach; however, it's not feasible as collecitng a thousand responses doesn't seem realistic. Here is where machine learning (ML) comes into play. I could train a model on sports betting events, using data such as bookmaker odds and actual outcomes, to predict probabilities of teams winning or losing. I could then use these model-predicted probabilities to identify value bets by comparing them to the bookmaker’s implied probabilities (derived from the odds).

Coincidentally, I happened to be discussing the moneyline odds (the sports betting jargon for wagering on a team's outright win) of a specific game with a friend: the Jets vs. the 49ers for American football. I mentioned that I didn't think the Jets were likely to beat the 49ers. He then replied, *"Well, what do you think is more likely? The Jets beating the 49ers or Tottenham uspetting Manchester City this weekend?"* The moneyline odds for the Jets winning were +162, while the odds for Tottenham upsetting Manchester City were +475. At that moment, his question sparked a realization. Perhaps a model leveraging data from different sports where similar “upset” scenarios occur might enhance a model’s ability to evaluate events like the Jets upsetting the 49ers. Conceptually, you could think of it like gaining additional perspectives to improve decision-making, similar to collecting a "wisdom of the crowd" for betting scenarios.

Thus, the plan for this project is to use ML to identify value bets and test its effectiveness as a betting strategy. Specifically, I aim to compare two models: one trained on data from multiple sports and another trained on data from a single sport. The key question is whether including multi-sport data improves the model’s performance on metrics like expected value (EV) and return on investment (ROI). 

## Data

Before describing the methodology, let me first provide some context about the data. 

## Methodology

## Results

## Conclusion