---
layout: page
title: Using ML to Identify Value Bets EDA
permalink: /pages/valuebets-part2
---

## Identifying Value Bets in Sports Using Machine Learning EDA

If you need to read [Part 1](/projects/valuebets).
<br>
Skip to [Part 2](/pages/valuebets-part3).
<br>
Check out the [Methodology](/pages/valuebets-method)

# Results

In this section, we are going to first understand our data.

#### Exploring the Data

Shape of the entire dataset: (157859, 43). 

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

Since we have many bookmakers, let's take a look at how spread out each bookmaker's home and away odds are, or their **average odds deviation**.

<br>
<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; margin-bottom: 10px;">Average Odds Deviation Summary</caption>
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
On average, bookmakers tend to favor the home team more than the average odds set by the platform Oddsportal as the mean average deviation for bookmakers' home odds are 5 lower than the average home odds.
Let's break this down by sport as shown below.

<br>
<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; margin-bottom: 10px;">Average Odds Deviation By Sport Summary</caption>
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
If we examine this by sport, it appears that the greatest deviations follow this order: basketball > American-football > soccer. 
We can also visualize the deviation by bookmakers (first) and by bookmaker and sport (second) below.

<br>
<div style="text-align: center;">
  <figure>
    <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/overall_deviation_bookmaker.png" alt="Average Odds Deviation by Bookmaker" width="600"/>
  </figure>
</div>

<br>

<div style="text-align: center;">
  <figure>
    <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_deviation_bookmakers.png" alt="Average Odds Deviation by Sport" width="950"/>
  </figure>
</div>

<br>
When comparing the three sports, some bookmakers consistently seem to be inflating both home and away team odds simultaneously. There doesn't appear to be any bookmakers inflating home odds while deflating away odds, or vice versa. Interestingly, for football/soccer, most bookmakers tend to deflate both home and away team odds by making them more positive (lower perceived likelihood for these events). 

---

On the topic of bookmakers, let's also look at **bookmaker margins**. This is the charge a bookmaker usually takes for setting the bet. The average margin for all bookmakers and games is 4% (SD = 0.6%). We can also compare the margin across sports shown below.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/avg_margin_by_bookmaker_sport.png" alt="Avg Margin by Bookmaker and Sport" width="600" /> </div>

---

Next, let's look at the **adjusted historical win rate variable** (i.e. win rate that is adjusted based on whether the team was favored or considered an underdog). For the entire dataset, the average home team adjusted historical win rate is 41% while the away team adjusted historical win rate is 38%. This may not be particularly meaningful to know because those two win rates are averaged across all the teams. Instead, what I'll show you is the adjusted historical win rate for my favorite sports teams.

<br>
<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; margin-bottom: 10px;">Adjusted Historical Home and Away Win Rate</caption>
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="border: 1px solid #ddd; padding: 8px;">Team</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Home Win Rate</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Away Win Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Los Angeles Lakers</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4139</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.3785</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">San Francisco 49ers</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4185</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.3801</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Tottenham</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4771</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4376</td>
    </tr>
  </tbody>
</table>
<br>

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

Are upsets more likely to occur for home or away teams? Surprisingly, what I found was that upsets are significantly more likely when a home underdog defeats a favored away team than the other way around. This could be some evidence to demonstrate "home court advantage"! The three asterisks (***) denote a p-value less than 0.001.

<br>
<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_upset.png" alt="Upset Frequency" width="900" /> </div>

---

We can also explore home and away score differentials. I calculated a **standardized home and away rolling score differential** for the last 5 games every team played at home or away. The overall average home rolling score differential is 0.009 while the average away rolling score differential is 0.017. The distributions of both appear quite similar.

<br>
<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_dist_roll_scorediff.png" alt="Distribution Rolling Home and Away Score Differential" width="900" /> </div>

---

Almost done exploring our features. We still need to examine **home and away team ELO ratings**. The overall average home ELO rating is 702.90 while the average away ELO rating is 702.33. I'll plot the distributions of the average home and away ELO ratings by sport across all the seasons.

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

---

Lastly, we can examine **similar home and away team ELO ratings**. The overall average similar home team ELO rating is 1239.34 while the overall average similar away team ELO rating is 1214.07. Again, I'll plot the distributions of the average similar home and away team ELO ratings by sport across all the seasons. 

<br>
<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/valuebets/grid_sim_elo_rating.png" alt="Distribution of SIMILAR Home and Away ELO Ratings" width="900" /> </div>

I'll also examine the average difference between the team ELO rating and the similar team ELO rating, just to get a sense of how the ELO rating differs from the "cross sport" ELO rating. The average difference between the home team ELO rating and the similar home team ELO rating is -536.44 whereas the average difference between Away team ELO rating and Similar Away team ELO rating is -511.74. 

---

If you need to read [Part 1](/projects/valuebets).
<br>
Skip to [Part 2](/pages/valuebets-part3).
<br>
Check out the [Methodology](/pages/valuebets-method)

<br>

