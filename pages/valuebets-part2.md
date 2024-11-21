---
layout: page
title: Using ML to Identify Value Bets Part 2
permalink: /pages/valuebets-part2
---

## Identifying Value Bets in Sports Using Machine Learning Part 2

Welcome back. If you need to read Part 1, [click here](/projects/valuebets).

# Results

Summary of the results here:

#### Exploring the Data

Shape of the entire dataset: (157859, 43). 

<br>
First let's check the distribution of the home and away probabilites. I'm plotting probabilities and not odds because probabilities are more intuitive to understand.
<br>
--- 
Since we have many bookmakers, let's take a look at how spread out each bookmaker's home and away odds are.

| Statistic       | Home Odds      | Away Odds      | Draw Odds      |
|-----------------|----------------|----------------|----------------|
| **Mean**        | -4.930794      | -0.844018      | 0.102749       |
| **Min**         | -7586.000000   | -6667.000000   | -506.000000    |
| **Max**         | 8333.000000    | 3334.000000    | 446.000000     |
| **Std Dev**     | 164.484117     | 64.243444      | 24.697895      |
<br>
Generally speaking, it seems that the average bookmaker favors both the home and away team more than the average odds reported by Oddsportal as indicated by the negative sign (remember negative means the favorite is more likely to win).
<br>

| Statistic                  | Home Odds Mean | Home Odds Min | Home Odds Max | Home Odds Std Dev | Away Odds Mean | Away Odds Min | Away Odds Max | Away Odds Std Dev | Draw Odds Mean | Draw Odds Min | Draw Odds Max | Draw Odds Std Dev |
|----------------------------|----------------|---------------|---------------|-------------------|----------------|---------------|---------------|-------------------|----------------|---------------|---------------|-------------------|
| **american-football**      | -2.0837        | -3333.0       | 3000.0        | 64.8964           | -0.4665        | -938.0        | 443.0         | 37.1820           | N/A            | N/A           | N/A           | N/A               |
|----------------------------|----------------|---------------|---------------|-------------------|----------------|---------------|---------------|-------------------|----------------|---------------|---------------|-------------------|
| **basketball**             | -7.5838        | -7586.0       | 8333.0        | 206.0938          | -1.2306        | -6667.0       | 3334.0        | 69.9314           | N/A            | N/A           | N/A           | N/A               |
|----------------------------|----------------|---------------|---------------|-------------------|----------------|---------------|---------------|-------------------|----------------|---------------|---------------|-------------------|
| **football**               | 0.2301         | -2083.0       | 917.0         | 45.0378           | -0.0620        | -1120.0       | 1114.0        | 64.6336           | 0.1027         | -506.0        | 446.0         | 24.6979           |
<br>
It seems like we see the greatest deviations in basketball > American-football > soccer. These descriptive stats may be difficult to interpret, so let's visualize this average deviation by bookmakers for the different odds types. 

<img src="assets/img/projects/valuebets/overall_deviation_bookmaker.png" alt="Overall Average Deviation Odds by Bookmaker" width="400" />

How do this look for the three sports?

<img src="assets/img/projects/valuebets/grid_deviation_bookmaker.png" alt="Sport Specific Deviation Odds by Bookmaker" width="400" />

plot team historical win rate adjusted for home and away overall and by sport
plot ELO ratings per team across seasons
look at ELO ratings for teams that have a higher than 50% historical win rate vs lower than 50% historical win rate
plot score differential for home and away teams -> is this a distribution?

frequency counts for categorical columns
insert table here.
are streaks more prevalent for certain teams or sports?
plot upsets analysis


correlation heatmap

Let's look at bookmaker margins.

#### Preparing the models

To start, let's take a look at the size of my datasets.
- Training dataset has the shape:
- Validation dataset has the shape:
- Test dataset has the shape:
- Holdout Bball dataset has the shape:

#### Model Results

# Conclusion