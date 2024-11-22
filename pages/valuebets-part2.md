---
layout: page
title: Using ML to Identify Value Bets Part 2
permalink: /pages/valuebets-part2
---

## Identifying Value Bets in Sports Using Machine Learning Part 2

If you need to read [Part 1](/projects/valuebets).
Skip to [Part 3](/pages/valuebets-part3).

# Results

In this section, we are going to first understand our data.

#### Exploring the Data

Shape of the entire dataset: (157859, 43). 

<br>
First let's examine the average home and away odds given by Oddsportal. This is our main dependent variable in our analysis later on.
<br>

<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
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
      <td style="border: 1px solid #ddd; padding: 8px;">-136.77</td>
      <td style="border: 1px solid #ddd; padding: 8px;">132.95</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Min</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-13333.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-6667.00</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Max</td>
      <td style="border: 1px solid #ddd; padding: 8px;">2413.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3685.00</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Standard Deviation</td>
      <td style="border: 1px solid #ddd; padding: 8px;">548.93</td>
      <td style="border: 1px solid #ddd; padding: 8px;">402.37</td>
    </tr>
  </tbody>
</table>

<br>
We can see that home teams are generally more favored than away teams. We can visualize the distribution of the average home and away odds as well.

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/overall_kde_avg_odds.png" alt="Overall Distribution of Odds Type" width="600" /> </div>

It appears that the distribution of home and away odds is bi-model, which makes sense given that there are usually teams that are favored to win when playing at home (e.g. top teams) and home teams that are underdogs (e.g. bottom teams). The same goes for teams when playing away. We also see a similar pattern if we break this down by sport.

<br>
<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="border: 1px solid #ddd; padding: 8px;">Sport</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Avg Home Odds Mean</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Avg Home Odds Min</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Avg Home Odds Max</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Avg Home Odds Std Dev</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Avg Away Odds Mean</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Avg Away Odds Min</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Avg Away Odds Max</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Avg Away Odds Std Dev</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>American Football</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">-132.64</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-8000.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">895.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">339.80</td>
      <td style="border: 1px solid #ddd; padding: 8px;">57.96</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-1600.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">1730.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">260.89</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>Basketball</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">-208.78</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-13333.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">1935.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">628.38</td>
      <td style="border: 1px solid #ddd; padding: 8px;">86.43</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-6667.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">2512.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">385.27</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>Football</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">72.43</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-2917.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">2413.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">356.94</td>
      <td style="border: 1px solid #ddd; padding: 8px;">335.60</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-1132.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3685.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">480.60</td>
    </tr>
  </tbody>
</table>

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_kde_odds.png" alt="Overall Distribution of Odds Type" width="950" /> </div>

--- 

Since we have many bookmakers, let's take a look at how spread out each bookmaker's home and away odds are.

<br>
<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
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
      <td style="border: 1px solid #ddd; padding: 8px;">-4.93</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.84</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.10</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>Min</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">-7586.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-6667.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-506.00</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>Max</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">8333.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3334.00</td>
      <td style="border: 1px solid #ddd; padding: 8px;">446.00</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;"><strong>Std Dev</strong></td>
      <td style="border: 1px solid #ddd; padding: 8px;">164.48</td>
      <td style="border: 1px solid #ddd; padding: 8px;">64.24</td>
      <td style="border: 1px solid #ddd; padding: 8px;">24.70</td>
    </tr>
  </tbody>
</table>

<br>
Generally speaking, it seems that the average bookmaker favors both the home and away team more than the average odds reported by Oddsportal as indicated by the negative sign (remember negative means the favorite is more likely to win).

<br>
<div style="text-align: center; margin: 0 auto;">
  <table style="border-collapse: collapse; margin: 0 auto; text-align: center; font-size: 12px; border: 1px solid #ddd;">
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
        <td style="border: 1px solid #ddd; padding: 8px;">-2.08</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-3333.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">3000.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">64.90</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-0.47</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-938.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">443.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">37.18</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;"><strong>basketball</strong></td>
        <td style="border: 1px solid #ddd; padding: 8px;">-7.58</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-7586.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">8333.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">206.09</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-1.23</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-6667.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">3334.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">69.93</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;"><strong>football</strong></td>
        <td style="border: 1px solid #ddd; padding: 8px;">0.23</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-2083.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">917.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">45.04</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-0.06</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-1120.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1114.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">64.63</td>
        <td style="border: 1px solid #ddd; padding: 8px;">0.10</td>
        <td style="border: 1px solid #ddd; padding: 8px;">-506.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">446.00</td>
        <td style="border: 1px solid #ddd; padding: 8px;">24.70</td>
      </tr>
    </tbody>
  </table>
</div>

<br>
It seems like we see the greatest deviations in basketball > American-football > soccer. These descriptive stats may be difficult to interpret, so let's visualize this average deviation by bookmakers for the different odds types.

<br>
<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/overall_deviation_bookmaker.png" alt="Overall Average Deviation Odds by Bookmaker" width="600" /> </div>

When comparing the three sports, some bookmakers consistently seem to be inflating both home and away team odds simultaneously. There doesn't appear to be any bookmakers inflating home odds while deflating away odds, or vice versa. Interestingly, for soccer, most bookmakers tend to deflate both home and away team odds by making them more positive (lower perceived likelihood for these events). 

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_deviation_bookmakers.png" alt="Sport Specific Deviation Odds by Bookmaker" width="950" /> </div>

---

On the topic of bookmakers, let's look at bookmaker margins. This is the charge a bookmaker usually takes for setting the bet. The average margin for all bookmakers and games is 4% (SD = 0.6%). We can also compare the margin across sports.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/avg_margin_by_bookmaker_sport.png" alt="Avg Margin by Bookmaker and Sport" width="600" /> </div>

---

Now let's check out every team's adjusted historical win rate (i.e. the win rate conditional on whether the team was favored or considered an underdog). 

Whats the summary stats for home and away. Show me favorite teams in each sport historical win rate home and away for fun.


---

It might also be interesting to examine win and loss streaks. How often does this happen overall? and by sport? and are streaks more prevalent for certain teams?

---

Another interesting variable to examine is upsets. 

---


rolling score differential mean
plot ELO ratings per team across seasons
look at ELO ratings for teams that have a higher than 50% historical win rate vs lower than 50% historical win rate
plot score differential for home and away teams -> is this a distribution?


---

Now that we have explored our data, let's move onto the modeling.

Continue to [Part 3](/pages/valuebets-part3).

<br>

