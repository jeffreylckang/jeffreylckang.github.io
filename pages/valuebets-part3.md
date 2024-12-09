---
layout: page
title: Using ML to Identify Value Bets Part 2
permalink: /pages/valuebets-part3
---

## Identifying Value Bets in Sports Using Machine Learning Part 2

Return to [Part 1](/projects/valuebets).
<br>
Return to [Part 2](/pages/valuebets-part2).

# Results

After exploring the variables and engineered features, let's now evaluate the proposed models. As a refresher, my plan is to compare four sets of model predictions for identifying value bets.  I'll use EV and ROI as the profitability metrics to make these evaluations. 

<br>

To start, let's take a look at the size of my datasets.
- Training dataset has the shape: (105391, 53)
- Validation dataset has the shape: (13175, 53)
- Test dataset has the shape: (13175, 53)
- Holdout basketball dataset has the shape: (17886, 53)

#### "Model 1": Baseline Adjusted Historical Win Rate

To start, I will calculate EV and ROI using the adjusted historical win rates for home and away teams to identify value bets. This approach serves as a simple baseline model to compare against the ML models. You can think of this baseline model as betting on your favorite team solely based on their historical home or away win rate when they were favored. Using this approach, we can first establish how effective this simple betting strategy is. 

<br>
Here is how we are going to identify value bets. Recall that a value bet is defined as a situation where the bettor believes or the model predicts that the odds/probabilites are more favorable than what the bookmaker has set. For example, if the model predicts a higher win probability for the home team than the implied probability derived from the bookmaker's set home odds, than that would be considered a value bet.

> Model estimated win probability > Bookmaker set win probability
> Model estimated win probability - Bookmaker set win probability > 0 

One more note. Recall that on the previous page we found that the average bookmaker margin of the entire dataset for home and away moneyline bets is around 4%. I'll round that number up to 5% and consider that as the margin to beat in terms of classifying value bets.

> Model estimated win probability - Bookmaker set win probability > 0.05

Using the adjusted historical win rate, how many value bets can we identify? Let's first examine the **test data set**. Among all home bets, 28% were classified as value bets using the adjusted historical home win rate. Similarly, 47% of all away bets were identified as value bets using the adjusted historical away win rate. Of the 28% home value bets, 10% were “correct” (i.e., the home team won), while 13% of away value bets were correct.

<br>
Next up, I'll evaluate profitability first in terms of a "per bet" strategy, by calculating the EV of home and away value bets. Normally to calculate EV, I would use the implied probabilities from the bookmakers' odds. However, if I do so, I would find an EV of 0 for all home and away bets. Why? This is because bookmakers set intentionally set the EV to zero. Instead, in this analysis I will calculate EV using the model estimated probability (derived from the adjusted historical win rate). This way I can estimate a theoretical EV, which can help us assess how profitable this strategy *could be*, even though it isn’t realistic since we can’t bet directly using model-estimated probabilities/odds.

<br>
For home value bets, the average EV for a $10 wager was $7.11, while betting on all home games regardless of value bet or not resulted in an average EV of $0.15. This shows that betting on a home value bet on average earns you more returns than blindly betting on a home bet. However, the EV of $7.11 for a $10 wager means you’re not losing money, but you’re still getting less value than your initial stake, making the strategy only marginally effective. For away value bets, the average EV was $11.14 compared to $3.85 for betting on all away games. We can see that away value bets outperform home value bets in terms of EV.

<br>
While EV measures the profitability of each individual bet, we can use ROI to evaluate the long-term viability of this model. The ROI for home value bets was -94%, compared to -99% for all home bets. Similarly, the ROI for away value bets was -97%, slightly better than the -98% for all away bets. These results suggest that betting on value bets almost seems equally as unprofitable as betting on actual home and away bets in the long-run. To conclude, it appears that using the adjusted historical win rate to identify value bets isn't an effective betting strategy.

<br>
In addition to profitability metrics, we can evaluate this model’s accuracy. Assuming an adjusted historical win rate above 50% predicts a home team win (coded as 1), and below 50% predicts an away team win (coded as 0), the model achieved a prediction accuracy of 51% for home games and 57% for away games. This indicates that the model isn’t particularly accurate in predicting game outcomes.

<br>
To further evaluate the model, we calculated the F1 score, which balances precision (the percentage of correctly predicted wins out of all predicted wins) and recall (the percentage of correctly predicted wins out of all actual wins). The F1 score for home predictions was 0.42, while for away predictions, it was 0.16. (Note: F1 scores are not percentages but range between 0 and 1.) These scores indicate that the model performs poorly in predicting game outcomes using adjusted historical win rates.

<br>
Here's a table that summarizes all this information.
<div style="text-align: center; margin: 0; padding: 0;">
  <h3 style="margin: 0; font-size: 14px;">Test Data Set Evaluation Using Adj Win Rates</h3>
</div>
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
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
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

<br>
Now lets evaluate the **holdout basketball data set** using the same approach. For brevity, I won't go through the results in detail and I will present the summary table of results instead. You can see that the results are similar to the test data set in terms of both profitability and accuracy.  

<div style="text-align: center; margin: 0; padding:0;">
  <h3 style="margin: 0; font-size: 14px;">Holdout Basketball Data Set Evaluation Using Adj Win Rates</h3>
</div>
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
      <td style="border: 1px solid #ddd; padding: 8px;">0.2239</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4302</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion Correct</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.0798</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.1192</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">5.5755</td>
      <td style="border: 1px solid #ddd; padding: 8px;">6.5975</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.8384</td>
      <td style="border: 1px solid #ddd; padding: 8px;">1.4696</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-90.80</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-98.19</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-101.06</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-102.84</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Accuracy (Threshold 0.5)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4963</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5731</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">F1 Score</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.3322</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.0000</td>
    </tr>
  </tbody>
</table>

#### Model 2: Random Forest

Now, let’s move on to the machine learning models! The first model we’ll explore is a RF model to estimate home and away team wins. We'll use these binary predictions and convert them into home and away team win probabilities. Here’s the procedure I used to develop the RF models:

1.	**Hyperparameter Tuning**: I tested various combinations of RF parameters, including the number of estimators (10, 20, 30, 40), maximum depth (3, 5, 10, 15), minimum samples required to split a node (10, 20), and minimum samples required per leaf (10, 20).
 
2.	**Evaluation Metric**: I used accuracy as the primary evaluation metric, as the goal was to predict the binary outcome of whether the home team won. This means the loss function I used to assess performance was binary cross-entropy loss.
 
3.	**Validation**: After identifying the best model through hyperparameter tuning, I evaluated its performance on the validation set to ensure it generalizes well to unseen data.
 
4.	**Testing**: Finally, I tested the best-performing model on the test dataset.

<br>
Here are the results for the test data set.

<h3 style="text-align: center; font-size: 14px; margin-bottom: 0px;">Test Data Set Evaluation Using RF</h3>
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
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion of Value Bets Overall</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5670</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4234</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4979</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.3625</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">7.6538</td>
      <td style="border: 1px solid #ddd; padding: 8px;">12.9445</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.1005</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.2775</td>
    </tr>
      <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-54.70</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.87</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-98.68</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-97.96</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Accuracy (Threshold 0.5)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8700</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8700</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">F1 Score</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8863</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8482</td>
    </tr>
  </tbody>
</table>

<br>
We can see that compared to the baseline model, the RF model classified significantly more home and away bets as value bets. Additionally, of the value bets, many of them were correct value bets (50% for home value bets and 36% for away value bets!). Both home and away value bets offer significantly higher EV than betting on all the home or away team bets. However, in the long-run, betting on the home and away value bets still doesn't seem like a viable strategy (despite the positive ROI of the away value bet) because the returns are negative or tiny (2%). In terms of accuracy and the F1 score, this RF model does significantly better in predicting game outcomes than the baseline model, so at least we see some improvement there.

<br>
Let's also examine the results for the holdout basketball data set. 

<h3 style="text-align: center; font-size: 14px; margin-bottom: 0px;">Holdout Basketball Data Set Evaluation Using RF</h3>
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
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion of Value Bets Overall</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5840</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4138</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5149</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.3578</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">7.5024</td>
      <td style="border: 1px solid #ddd; padding: 8px;">12.7874</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.2437</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.5696</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-53.68</td>
      <td style="border: 1px solid #ddd; padding: 8px;">1.68</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-101.06</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-102.84</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Accuracy (Threshold 0.5)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8749</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8749</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">F1 Score</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8921</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8512</td>
    </tr>
  </tbody>
</table>

<br>
We can see similar results as the test data set. Interestingly, we find a positive ROI for away value bets, which suggests that using a RF model to identify away value bets can be profitable. Overall, the RF model appears to offer significant improvement when it comes to profitability compared to the baseline model of using adjusted historical team win rates. In addition, the accuracy and F1 scores have significantly improved.

---

<br>
Instead of estimating probabilites, I can also use the RF model to estimate EV directly. This might produce results that are better optimized for profitability rather than probability. The procedure that follows is exactly the same, except now the dependent variable to estimate is the EV of the home and away bets (this also means that we will be using the root mean squared error or RMSE) as the loss function. 

<h3 style="text-align: center; font-size: 14px; margin-bottom: 10px;">Test Data Set Evaluation Using RF (EV)</h3>
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
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion of Value Bets Overall</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5518</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4259</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5511</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4257</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3.4689</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.6233</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-2.2680</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-3.6058</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-20.68</td>
      <td style="border: 1px solid #ddd; padding: 8px;">36.10</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-98.68</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-97.96</td>
    </tr>
  </tbody>
</table>

<h3 style="text-align: center; font-size: 14px; margin-bottom: 10px;">Holdout Basketball Data Set Evaluation Using RF (EV)</h3>
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
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion of Value Bets Overall</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5595</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4240</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5588</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4232</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3.3833</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.6685</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-2.2201</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-3.6820</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-27.95</td>
      <td style="border: 1px solid #ddd; padding: 8px;">27.96</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-101.06</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-102.84</td>
    </tr>
  </tbody>
</table>

<br>
These results reveal a clear pattern: for both the test data set and the holdout basketball data set, away value bets are more profitable than home value bets in terms of both EV and ROI. Notably, the positive ROI for away value bets suggests that betting on them, as identified by the RF EV model, can yield profits! However, the average EV for both home and away value bets is lower than in the RF model which estimated probabilities. Although I'm not sure why this occurs, I believe this discrepancy isn’t a major concern, as the calculated EVs are theoretical. Lastly, accuracy and F1 scores are not presented for these tables, as these metrics are less meaningful when estimating EV directly. This is because deriving implied win probabilities from EV assumes reliance on the bookmaker’s odds, which may not hold true in this context.

#### Model 3: Neural Network on single sport

Can a more complex ML model outperform the RF models in profitability? To test this, I trained a simple neural network (NN) model on basketball data. The NN consists of 3 hidden layers and a softmax activation output layer for predicting binary outcomes (home team win or loss) using binary cross-entropy as the loss function. I employed 5-fold cross-validation during training, with data processed in batches over 20 epochs. Hyperparameters, including dropout rate, learning rate, and weight decay, were tuned to optimize performance. Once trained, I evaluated the model with the best hyperparameters on the validation set and then on the test set. For brevity, I’ll present the NN model’s results on the holdout basketball data to compare with the previous models.

<h3 style="text-align: center; font-size: 14px; margin-bottom: 10px;">Holdout Basketball Data Set Evaluation Using NN</h3>
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
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion of Value Bets Overall</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5696</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4274</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5696</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4260</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">7.2599</td>
      <td style="border: 1px solid #ddd; padding: 8px;">12.6905</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.1144</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.2812</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-27.17</td>
      <td style="border: 1px solid #ddd; padding: 8px;">26.93</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-101.06</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-102.84</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Accuracy (Threshold 0.5)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9979</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9979</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">F1 Score</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9981</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9975</td>
    </tr>
  </tbody>
</table>

<br>
The NN model trained on basketball data shows similar patterns to the RF models. Away value bets outperform home value bets, with an EV exceeding $10 (indicating a return greater than the initial wager) and a positive ROI. Notably, the ROI for away value bets is significantly higher than the equivalent RF model (1.68%) and comparable to the ROI of the RF model estimating EV (27.96%). Additionally, the high accuracy and F1 scores suggest this NN model performs well in predicting game outcomes.

---

We can also train the NN model to estimate home and away EV bets directly as well. As mentioned above, this approach uses the same procedure, except the loss function switches to RMSE.

<h3 style="text-align: center; font-size: 14px; margin-bottom: 10px;">Holdout Basketball Data Set Evaluation Using NN (EV)</h3>
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
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion of Value Bets Overall</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5706</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4282</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5701</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4268</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3.4767</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.8183</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">1.9852</td>
      <td style="border: 1px solid #ddd; padding: 8px;">2.0636</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-27.39</td>
      <td style="border: 1px solid #ddd; padding: 8px;">26.87</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-101.06</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-102.84</td>
    </tr>
  </tbody>
</table>

<br>
The NN model estimating EV produces results similar to the previous models, with away value bets outperforming home value bets in both EV and ROI. The away value bet ROI (26.87%) closely aligns with the NN model predicting game outcomes (26.93%) and the RF model estimating EV (27.96%). Like the RF model, the theoretical EVs are less than the $10 initial wager. In conclusion, the NN model trained on basketball data seems to effectively identiff profitable away value bets. Because the NN model's performance on profitability is comparable to the RF model estimating EV, it may be preferable to use the RF model due to its lower computational complexity and ease of use.

#### Model 4: Neural Collaborative Filtering Neural Network

If you recall the primary motivation for this project, I set out to test whether a complex ML model leveraging cross-sport information could effectively identify profitable value bets. To do this, I developed a Neural Collaborative Filtering (NCF) model inspired by recommendation systems like those used by Netflix. These models identify relationships between users and items using embeddings, and I applied a similar approach here by embedding “game features” to identify similarities between teams across different sports. By embedding game features, the model can learn non-linear interactions between teams and games within a shared dimensional space.

<br>
Similar to the previous NN model, this NCF model architecture includes three hidden layers, a softmax activation function for the final output layer, binary cross-entropy as the loss function, and the same tuning for the hyperparameters. However, this NCF model also includes a sport-specific output layer which produces separate home and away win predictions for each sport to account for sport-specific nuances. 

<h3 style="text-align: center; font-size: 14px; margin-bottom: 10px;">Test Data Set Evaluation Using NCF</h3>
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
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion of Value Bets Overall</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5595</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4298</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5586</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4298</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">7.9571</td>
      <td style="border: 1px solid #ddd; padding: 8px;">13.5758</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.1811</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.1567</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-20.59</td>
      <td style="border: 1px solid #ddd; padding: 8px;">36.44</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-98.68</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-97.96</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Accuracy (Threshold 0.5)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9987</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9987</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">F1 Score</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9989</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9985</td>
    </tr>
  </tbody>
</table>

<h3 style="text-align: center; font-size: 14px; margin-bottom: 10px;">Holdout Basketball Data Set Evaluation Using NCF</h3>
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
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion of Value Bets Overall</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5780</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4181</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5702</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4175</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">7.2180</td>
      <td style="border: 1px solid #ddd; padding: 8px;">12.7158</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.0490</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.4317</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-29.49</td>
      <td style="border: 1px solid #ddd; padding: 8px;">28.09</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-101.06</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-102.84</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Accuracy (Threshold 0.5)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9908</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9908</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">F1 Score</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9920</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9891</td>
    </tr>
  </tbody>
</table>

<br>
By now it should be no surprise that away value bets consistently outperform home value bets in both EV and ROI. This pattern holds for both the test set and the holdout basktball dataset. This NCF model achieves the highest ROI for away value bets, with 36.44% on the test set data and 28.09% on the holdout basketball data--outperforming all previous models. Lastly, this NCF model demonstrates high accuracy and F1 scores on both datasets, indicating strong performance in predicting game outcomes.

---

Finally, let's investigate the EV and ROI metrics on this NCF model when it is trained to predict EV.

<h3 style="text-align: center; font-size: 14px; margin-bottom: 10px;">Test Data Set Evaluation Using NCF (EV)</h3>
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
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion of Value Bets Overall</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5738</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4401</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5652</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4310</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3.2040</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.5448</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for All Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">1.8398</td>
      <td style="border: 1px solid #ddd; padding: 8px;">2.0025</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-24.43</td>
      <td style="border: 1px solid #ddd; padding: 8px;">31.26</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-98.68</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-97.96</td>
    </tr>
  </tbody>
</table>

<h3 style="text-align: center; font-size: 14px; margin-bottom: 10px;">Holdout Basketball Data Set Evaluation Using NCF (EV)</h3>
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
      <td style="border: 1px solid #ddd; padding: 8px;">Proportion of Value Bets Overall</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5835</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4388</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5725</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4268</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3.1461</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.3856</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for All Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">1.8381</td>
      <td style="border: 1px solid #ddd; padding: 8px;">1.9268</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-30.63</td>
      <td style="border: 1px solid #ddd; padding: 8px;">21.40</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-101.06</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-102.84</td>
    </tr>
  </tbody>
</table>

<br>
As usual, we observe a similar pattern of results. While the ROI for away value bets is lower than that of the NCF model estimating game outcomes (probabilities), it remains positive. Taking the results altogether, it appears that the most profitable betting model is the NCF model.

# Conclusion

Now that we've made it to the end, let's reflect on the results!

<br>
The goal of this project was to develop a ML model capable of evaluating moneyline sports bets to identify profitable bets. I tested four statistical models that predicted either game outcomes (home team win or loss) or expected value. Using these predictions, I identified value bets--bets where the model's predicted probability or EV exceeded the bookmaker's implied probability or EV.

<br>
Among all the models, the NCF model leveraging cross-sport information delivered the best results in terms of profitability. It produced the highest ROI for away value bets compared to other models. Thus, this NCF model could then potentially be used to predict future home and away value bets, helping to identify which bets are worth placing. Overall, it seems that betting on away value bets is more profitable than betting on home value bets. The theoretical EVs were consistently higher for away value bets, meaning that on average, betting on away value bets yields higher returns than betting on home value bets.

<br>
Why might this be the case? Why are away value bets more profitable than home value bets? Well one possible reason could be due to upsets. We know that upsets tend to have higher payouts than regular bets (because underdogs are not favored to win), and away value bets may include more away upsets than home value bets including home upsets. Indeed, I found this to be the case for the NCF model's predictions on the holdout basketball dataset (with similar patterns seen in the test dataset). *50%* of correct away value bets were upsets compared to only *24%* of correct home value bets were upsets. Since upsets offer larger payouts, they may be enough to cover the losses.

<br>
In addition to upsets, the distribution of payouts may differ between away and home value bets. To see this, I can classify value bets into categories of low (<33%), medium (between 33% and 67%), or high (>67%) win probability and analyze their average payouts. The table below illustrates this distribution.

<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; margin-bottom: 10px;">Summary of Home and Away Value Bets by Probability Category For Holdout Basketball Dataset</caption>
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="border: 1px solid #ddd; padding: 8px;">Probability Category</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Avg Payout (Home)</th>
      <th style="border: 1px solid #ddd; padding: 8px;">% Bets (Home)</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Avg Payout (Away)</th>
      <th style="border: 1px solid #ddd; padding: 8px;">% Bets (Away)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Low (&lt;33%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">31.16</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.41%</td>
      <td style="border: 1px solid #ddd; padding: 8px;">34.35</td>
      <td style="border: 1px solid #ddd; padding: 8px;">15.90%</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Medium (33-67%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">9.24</td>
      <td style="border: 1px solid #ddd; padding: 8px;">49.49%</td>
      <td style="border: 1px solid #ddd; padding: 8px;">10.73</td>
      <td style="border: 1px solid #ddd; padding: 8px;">61.88%</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">High (&gt;67%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">2.92</td>
      <td style="border: 1px solid #ddd; padding: 8px;">46.10%</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3.16</td>
      <td style="border: 1px solid #ddd; padding: 8px;">22.21%</td>
    </tr>
  </tbody>
</table>

<br>
For away value bets, the majority of correct bets (62%) have a medium probability of winning with an average payout of $10.73--slightly higher than the inital $10 wager. Since most away value bets have this payout, we can reasonably conclude that the ROI would be positive. In contrast, home value bets have a different distribution. The majority of correct home value bets fall into the medium (50%) or high (46%) probability categories, with their average payouts at $9.42 and $2.92. These lower payouts likely contribute to the negative ROI for home value bets, especially considering that almost half of the home value bets are of the high probability, low payout type. Thus, the payouts may be insufficient to offset the losses from incorrect home value bets.

<br>
There we have it! Hopefully you learned something too reading this and maybe these insights could help you place some better bets!
If you have any feedback, questions, or ideas, please reach out to me -- I'd love to discuss anything related to this project further!

### Thanks for reading!

