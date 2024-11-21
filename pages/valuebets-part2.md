---
layout: page
title: Using ML to Identify Value Bets Part 2
permalink: /pages/valuebets-part2
---

## Identifying Value Bets in Sports Using Machine Learning Part 2

If you need to read [Part 1](/projects/valuebets).
Skip to [Part 3](/pages/valuebets-part3).

# Results

Summary of the results here:

#### Exploring the Data

Shape of the entire dataset: (157859, 43). 

<br>
First let's examine the average home and away odds given by Oddsportal. This is our main dependent variable in our analysis later on.
<br>

<table style="border-collapse: collapse; width: 80%; text-align: center; margin: 0 auto;">
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="border: 1px solid #ddd; padding: 8px;">Statistic</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Average Home Odds</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Average Away Odds</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-136.770389</td>
      <td style="border: 1px solid #ddd; padding: 8px;">132.954518</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Min</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-13333.000000</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-6667.000000</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Max</td>
      <td style="border: 1px solid #ddd; padding: 8px;">2413.000000</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3685.000000</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Standard Deviation</td>
      <td style="border: 1px solid #ddd; padding: 8px;">548.929175</td>
      <td style="border: 1px solid #ddd; padding: 8px;">402.370724</td>
    </tr>
  </tbody>
</table>

<br>
Let's visualize the distribution of the average home and away odds.

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/overall_kde_avg_odds.png" alt="Overall Distribution of Odds Type" width="600" /> </div>

--- 

Since we have many bookmakers, let's take a look at how spread out each bookmaker's home and away odds are.

<br>
<table style="border-collapse: collapse; width: 80%; text-align: center; margin: 0 auto;">
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="border: 1px solid #ddd; padding: 8px;"></th>
      <th style="border: 1px solid #ddd; padding: 8px;">Home Odds</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Away Odds</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Draw Odds</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>Mean</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">-4.930794</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.844018</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.102749</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>Min</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">-7586.000000</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-6667.000000</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-506.000000</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>Max</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">8333.000000</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3334.000000</td>
      <td style="border: 1px solid #ddd; padding: 8px;">446.000000</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>Std Dev</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">164.484117</td>
      <td style="border: 1px solid #ddd; padding: 8px;">64.243444</td>
      <td style="border: 1px solid #ddd; padding: 8px;">24.697895</td>
    </tr>
  </tbody>
</table>
<br>
Generally speaking, it seems that the average bookmaker favors both the home and away team more than the average odds reported by Oddsportal as indicated by the negative sign (remember negative means the favorite is more likely to win).-

<br>
<table style="border-collapse: collapse; width: 90%; text-align: center; margin: 20px auto; font-size: 14px; border: 1px solid #ddd;">
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="border: 1px solid #ddd; padding: 8px;"></th>
      <th style="border: 1px solid #ddd; padding: 8px;">Home Odds Mean</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Home Odds Min</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Home Odds Max</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Home Odds Std Dev</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Away Odds Mean</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Away Odds Min</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Away Odds Max</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Away Odds Std Dev</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Draw Odds Mean</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Draw Odds Min</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Draw Odds Max</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Draw Odds Std Dev</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>american-football</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">-2.0837</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-3333.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3000.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">64.8964</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.4665</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-938.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">443.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">37.1820</td>
      <td style="border: 1px solid #ddd; padding: 8px;">N/A</td>
      <td style="border: 1px solid #ddd; padding: 8px;">N/A</td>
      <td style="border: 1px solid #ddd; padding: 8px;">N/A</td>
      <td style="border: 1px solid #ddd; padding: 8px;">N/A</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>basketball</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">-7.5838</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-7586.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">8333.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">206.0938</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-1.2306</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-6667.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3334.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">69.9314</td>
      <td style="border: 1px solid #ddd; padding: 8px;">N/A</td>
      <td style="border: 1px solid #ddd; padding: 8px;">N/A</td>
      <td style="border: 1px solid #ddd; padding: 8px;">N/A</td>
      <td style="border: 1px solid #ddd; padding: 8px;">N/A</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>football</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.2301</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-2083.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">917.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">45.0378</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.0620</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-1120.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">1114.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">64.6336</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.1027</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-506.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">446.0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">24.6979</td>
    </tr>
  </tbody>
</table>
<br>
It seems like we see the greatest deviations in basketball > American-football > soccer. These descriptive stats may be difficult to interpret, so let's visualize this average deviation by bookmakers for the different odds types. 

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/overall_deviation_bookmaker.png" alt="Overall Average Deviation Odds by Bookmaker" width="600" /> </div>

How do this compare for the three sports?

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_deviation_bookmakers.png" alt="Sport Specific Deviation Odds by Bookmaker" width="950" /> </div>

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

Now that we have explored our data, let's move onto the modeling.

Continue to [Part 3](/pages/valuebets-part3).

<br>

