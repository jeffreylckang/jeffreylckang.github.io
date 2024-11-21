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

--- 

Since we have many bookmakers, let's take a look at how spread out each bookmaker's home and away odds are.
<br>
<div style="text-align: center; margin: 0 auto;">
  <table style="margin: 0 auto;">
    <thead>
      <tr>
        <th></th>
        <th>Home Odds</th>
        <th>Away Odds</th>
        <th>Draw Odds</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><strong>Mean</strong></td>
        <td>-4.930794</td>
        <td>-0.844018</td>
        <td>0.102749</td>
      </tr>
      <tr>
        <td><strong>Min</strong></td>
        <td>-7586.000000</td>
        <td>-6667.000000</td>
        <td>-506.000000</td>
      </tr>
      <tr>
        <td><strong>Max</strong></td>
        <td>8333.000000</td>
        <td>3334.000000</td>
        <td>446.000000</td>
      </tr>
      <tr>
        <td><strong>Std Dev</strong></td>
        <td>164.484117</td>
        <td>64.243444</td>
        <td>24.697895</td>
      </tr>
    </tbody>
  </table>
</div>
<br>
Generally speaking, it seems that the average bookmaker favors both the home and away team more than the average odds reported by Oddsportal as indicated by the negative sign (remember negative means the favorite is more likely to win).
<br>
<div style="text-align: center; margin: 0 auto;">
  <table style="margin: 0 auto; border-collapse: collapse; width: 100%; text-align: center;">
    <thead>
      <tr>
        <th></th>
        <th>Home Odds Mean</th>
        <th>Home Odds Min</th>
        <th>Home Odds Max</th>
        <th>Home Odds Std Dev</th>
        <th>Away Odds Mean</th>
        <th>Away Odds Min</th>
        <th>Away Odds Max</th>
        <th>Away Odds Std Dev</th>
        <th>Draw Odds Mean</th>
        <th>Draw Odds Min</th>
        <th>Draw Odds Max</th>
        <th>Draw Odds Std Dev</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><strong>american-football</strong></td>
        <td>-2.0837</td>
        <td>-3333.0</td>
        <td>3000.0</td>
        <td>64.8964</td>
        <td>-0.4665</td>
        <td>-938.0</td>
        <td>443.0</td>
        <td>37.1820</td>
        <td>N/A</td>
        <td>N/A</td>
        <td>N/A</td>
        <td>N/A</td>
      </tr>
      <tr>
        <td><strong>basketball</strong></td>
        <td>-7.5838</td>
        <td>-7586.0</td>
        <td>8333.0</td>
        <td>206.0938</td>
        <td>-1.2306</td>
        <td>-6667.0</td>
        <td>3334.0</td>
        <td>69.9314</td>
        <td>N/A</td>
        <td>N/A</td>
        <td>N/A</td>
        <td>N/A</td>
      </tr>
      <tr>
        <td><strong>football</strong></td>
        <td>0.2301</td>
        <td>-2083.0</td>
        <td>917.0</td>
        <td>45.0378</td>
        <td>-0.0620</td>
        <td>-1120.0</td>
        <td>1114.0</td>
        <td>64.6336</td>
        <td>0.1027</td>
        <td>-506.0</td>
        <td>446.0</td>
        <td>24.6979</td>
      </tr>
    </tbody>
  </table>
</div>
<br>
It seems like we see the greatest deviations in basketball > American-football > soccer. These descriptive stats may be difficult to interpret, so let's visualize this average deviation by bookmakers for the different odds types. 

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/overall_deviation_bookmaker.png" alt="Overall Average Deviation Odds by Bookmaker" width="600" /> </div>

How do this compare for the three sports?

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_deviation_bookmakers.png" alt="Sport Specific Deviation Odds by Bookmaker" width="1200" /> </div>

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