---
layout: page
title: Using ML to Identify Value Bets Part 2
permalink: /pages/valuebets-part3
---

## Identifying Value Bets in Sports Using Machine Learning Part 2

Return to [Part 1](/projects/valuebets).
Return to [Part 2](/pages/valuebets-part2).

# Results

After exploring our variables and engineered features, let's now evaluate our proposed models. As a refresher, my proposed procedure is to compare four sets of model predictions for identifying value bets.  I'll use EV and ROI as the profitability metrics to make these evaluations. 

<br>

To start, let's take a look at the size of my datasets.
- Training dataset has the shape: (105391, 53)
- Validation dataset has the shape: (13175, 53)
- Test dataset has the shape: (13175, 53)
- Holdout Bball dataset has the shape: (17886, 53)

#### "Model 1": Baseline Adjusted Historical Win Rate

To start, I will calculate EV and ROI using the adjusted historical win rates for home and away teams to identify value bets. This approach serves as a simple baseline model to compare against the ML models. The idea behind this is that the ML models should outperform this baseline model in identifying value bets. You can think of this baseline model as betting on your favorite team solely based on their historical home or away win rate when they were favored. Using this approach, we can first establish how effective a simple betting strategy is. 
<br>

How do we classify which home or away moneyline bets are considered value bets? Recall that a value bet is defined as a situation where the bettor believes or the model predicts that the odds/probabilites are more favorable than what the bookmaker has set. For example, if the model predicts a higher win probability for the home team than the implied probability derived from the bookmaker's set home odds, than that would be considered a value bet.

> Model estimated win probability > Bookmaker set win probability

OR 

> Model estimated win probability - Bookmaker set win probability > 0 

<br>
One more note. Recall that on the previous page we found that the average bookmaker margin of the entire dataset for home and away moneyline bets is around 4%. I'll round that number up to 5% and consider that as the margin to beat in terms of classifying value bets.

> Model estimated win probability - Bookmaker set win probability > 0.05

<br>
So how many value bets were identified using this strategy? Among all home bets, 28% were classified as value bets based on the adjusted historical home win rate for each team. Similarly, 47% of all away bets were identified as value bets using the adjusted historical away win rate. Of the 28% home value bets, 10% were “correct” (i.e., the home team won), while 13% of away value bets were correct.

<br>
Next up, I'll evaluate profitability first in terms of a "per bet" strategy, by calculating the EV of home and away value bets. Normally to calculate EV, I would use the implied probabilities from the bookmakers' odds. However, in this analysis I will calculate it using the adjusted historical win rate because bookmakers set moneyline odds with an EV of zero, meaning any EV calculation using their probabilities would always result in zero. By using the adjusted win rates instead, I can estimate a theoretical EV, which can help us assess how profitable this strategy *could be*, even though it isn’t realistic since we can’t bet directly using model-estimated probabilities/odds.

<br>
For home value bets, the average EV for a $10 wager was $7.11, while betting on all home games regardless of value resulted in an average EV of $0.15. This shows that identifying value bets seems to be a better strategy than betting on all home games. However, an average EV of $7.11 per $10 wager means you’re not losing money, but you’re still getting less value than your initial stake, making the strategy only marginally effective. For away value bets, the average EV was $11.14 compared to $3.85 for betting on all away games. We can see that away value bets outperform home value bets in terms of EV.

<br>
While EV measures the profitability of each individual bet, we can use ROI to evaluate the long-term viability of this model using the adjusted historical win rate. The ROI for home value bets was -94%, compared to -99% for all home bets. Similarly, the ROI for away value bets was -97%, slightly better than the -98% for all away bets. These results suggest that betting on value bets almost seems equally as unprofitable as betting on actual home and away bets. To conclude, it appears that using the adjusted historical win rate to identify value bets isn't an effective betting strategy.

<br>
In addition to profitability metrics, we can evaluate this model’s accuracy. Assuming an adjusted historical win rate above 50% predicts a home team win (binary outcome of 1), and below 50% predicts an away team win (binary outcome of 0), the model achieved a prediction accuracy of 51% for home games and 57% for away games. This indicates that the model isn’t particularly accurate in predicting game outcomes.

To further evaluate the model, we calculated the F1 score, which balances precision (the percentage of correctly predicted wins out of all predicted wins) and recall (the percentage of correctly predicted wins out of all actual wins). The F1 score for home predictions was 0.42, while for away predictions, it was 0.16. (Note: F1 scores are not percentages but range between 0 and 1.) These scores indicate that the model performs poorly in predicting game outcomes using adjusted historical win rates.

Here's a table that summarizes this information.
<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="border: 1px solid #ddd; padding: 8px;">Metric</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Home</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Away</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion Overall</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.2838</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4744</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion Correct</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.0967</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.1259</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">7.7117</td>
      <td style="border: 1px solid #ddd; padding: 8px;">11.1423</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for All Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.1504</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3.8540</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-93.53</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-97.47</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-98.68</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-97.98</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Accuracy (Threshold 0.5)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5129</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5668</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">F1 Score</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4160</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.1569</td>
    </tr>
  </tbody>
</table>

#### Model 2: Random Forest

#### Model 3: Feedforward Neural Network on single sport

#### Model 4: Neural Collaborative Filtering Neural Network

# Conclusion

