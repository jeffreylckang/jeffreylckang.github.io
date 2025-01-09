---
layout: page
title: Using ML to Identify Value Bets EDA
permalink: /pages/valuebets-part2
---

## Identifying Value Bets in Sports Using Machine Learning EDA

If you need to read [Part 1](/projects/valuebets).
<br>
Go to [Part 2](/pages/valuebets-part3).
<br>
Check out the [Methodology](/pages/valuebets-method)

# Exploring the Data

In this section, let's explore the data. The total number of rows in this data set is 149,627. 

<br>

First let's examine the **average home and away odds** given by Oddsportal.
<br>

<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; margin-bottom: 10px;">Average Home and Away Odds Summary</caption>
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
Generally speaking, home teams are more favored than away teams. Let's visualize the distribution of the average home and away odds as well.

<div style="text-align: center;">
  <figure>
    <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/overall_kde_avg_odds.png" alt="Distribution of Home and Away Odds" width="600"/>
  </figure>
</div>

It appears that the distribution of home and away odds is bi-model, which makes sense given that there are usually teams that are favored to win when playing at home (e.g. top teams) and home teams that are underdogs (e.g. bottom teams). The same goes for teams when playing away. We also see a similar pattern if we break this down by sport.

<br>
<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; margin-bottom: 10px;">Average Home and Away Odds By Sport Summary</caption>
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

<div style="text-align: center;">
  <figure>
    <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_kde_odds.png" alt="Distribution of Home and Away Odds by Sport" width="950" />
  </figure>
</div>

--- 

It's important to remember that in every sportsbook context, bookmakers set **bookmaker margins**. This is the charge a bookmaker usually takes for setting the bet. The average margin for all bookmakers and games in my collected data is about 4% (SD = 0.6%). We can also compare the margin across sports shown below.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/avg_margin_by_bookmaker_sport.png" alt="Avg Margin by Bookmaker and Sport" width="600" /> </div>

---

It might also be interesting to examine **win and loss streaks**. In the table, we can see that out of all the games, home and away win streaks represent about 14% of the games whereas home and away loss streaks represent about 7% of the games.

<br>
<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; margin-bottom: 10px;">Average Win and Loss Streaks</caption>
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="border: 1px solid #ddd; padding: 8px;">Statistic</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Mean Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Home Win Streak</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.14</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Away Win Streak</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.14</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Home Loss Streak</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.08</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Away Loss Streak</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.07</td>
    </tr>
  </tbody>
</table>

<br>
I was also curious to know which teams have the most win and loss streaks. So across all three sports, Manchester City holds the highest win streak percentage, with 47% of their home games as part of a streak and 43% of their away games as part of a streak. On the other hand, Sheffield United leads in loss streaks, with 31% of their home games as part of a losing streak and 33% of their away games as part of a losing streak. Go figure.
<br>

---

Another interesting variable to examine is **upset frequency**. Since this variable is binary, grouping it by season allows me to calculate the proportion of upsets for each season. We can see that this number appears to be around 30% for all sports give or take.

<br>
<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_upset_freq.png" alt="Upset Frequency" width="900" /> </div>

Are upsets more likely to occur for home or away teams? Surprisingly, what I found was that upsets are significantly more likely when an away underdog defeats a favored home team compared to the other way around. The three asterisks (***) denote a p-value less than 0.001.

<br>
<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_upset.png" alt="Upset Frequency" width="900" /> </div>

---

Let's also take a look at **home and away team ELO ratings**. The overall average home ELO rating is 702.90 while the average away ELO rating is 702.33. I'll plot the distributions of the average home and away ELO ratings by sport across all the seasons.

<br>
<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_elo_rating.png" alt="Distribution of Home and Away ELO Ratings" width="900" /> </div>

<br>
What might be interesting to know is the average home and away ELO ratings for teams with a higher than 50% adjusted historical win rate vs lower than 50% adjusted historical win rate.

<br>
<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; margin-bottom: 10px;">Average Home and Away ELO Ratings</caption>
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="border: 1px solid #ddd; padding: 8px;">Metric</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Average ELO Rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Home Team ELO > 50% Adjusted Home Win Rate</td>
      <td style="border: 1px solid #ddd; padding: 8px;">927.26</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Home Team ELO < 50% Adjusted Home Win Rate</td>
      <td style="border: 1px solid #ddd; padding: 8px;">627.96</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Away Team ELO > 50% Adjusted Away Win Rate</td>
      <td style="border: 1px solid #ddd; padding: 8px;">1351.75</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Away Team ELO < 50% Adjusted Away Win Rate</td>
      <td style="border: 1px solid #ddd; padding: 8px;">653.10</td>
    </tr>
  </tbody>
</table>
<br>

With that, we can move onto the analysis [Part 2](/pages/valuebets-part3).
<br>

If you need to read [Part 1](/projects/valuebets).
<br>
Check out the [Methodology](/pages/valuebets-method)

<br>

