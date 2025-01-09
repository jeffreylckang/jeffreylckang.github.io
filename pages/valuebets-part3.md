---
layout: page
title: Using ML to Identify Value Bets Part 2
permalink: /pages/valuebets-part3
---

## Identifying Value Bets in Sports Using Machine Learning Part 2

Return to [Part 1](/projects/valuebets).
<br>
Go to [Methodology](/pages/valuebets-method).
<br>
Go to [Exploratory Data Analysis](/pages/valuebets-part2).
<br>
[Use my model to help identify value bets!](https://value-bet-model-app.onrender.com/)

# Analysis and Results

The data will be split as follows:
- 80% Training
- 10% Validation
- 10% Test

The features I used to train the model is detailed in the Methodology section. 

<br>

Let's take a look at the size of my datasets.
- Training dataset has the shape: (105391, 53)
- Validation dataset has the shape: (13175, 53)
- Test dataset has the shape: (13175, 53)

#### Model 1: Random Forest

The first machine learning model I will employ to identify moneyline value bets is a Random Forest model. I'm going to use the RF model to estimate a binary home team win outcome, where a 1 will indicate if the home team won and a 0 will indicate if the home team lost (and therefore the away team won). I'll then use the binary predictions and convert them into probabilities. Using these predicted probabilities, I can then identify moneyline home and away value bets. 

### Identifying value bets
Recall that a value bet is defined as a situation where the bettor or model predicts that the odds/probabilites are more favorable than what the bookmaker has set. For example, if the model predicts a higher win probability for the home team than the implied probability derived from the bookmaker's set home odds, then we would consider that a value bet.

> Model estimated win probability > Bookmaker win probability
> 
> OR
> 
> Model estimated win probability - Bookmaker win probability > 0 

If we follow the formula above, we can identify value bets. However, it's likely that in the long-run of making multiple bets that we end up net-negative. This is because bookmakers don't set entirely accurate odds. Bookmakers set odds that include a bookmaker margin, which is the amount that they earn when setting bets. In the exploratory data analysis, I found that the average bookmaker margin of the entire dataset for home and away moneyline bets is around 4%. I'll round that number up to 5% and consider that as the margin to beat in terms of classifying value bets.

> Model estimated win probability - Bookmaker set win probability > 0.05

### Procedure

Here's the procedure I used to develop the RF model.
1.	**Hyperparameter Tuning**: I tested various combinations of RF parameters, including the number of estimators (10, 20, 30, 40), maximum depth (3, 5, 10, 15), minimum samples required to split a node (10, 20), and minimum samples required per leaf (10, 20).
 
2.	**Evaluation Metric**: I used accuracy as the primary evaluation metric, as the goal was to predict the binary outcome of whether the home team won. This means the loss function I used to assess performance was binary cross-entropy loss.
 
3.	**Validation**: After identifying the best model through hyperparameter tuning, I evaluated its performance on the validation set to ensure it generalizes well to unseen data.
 
4.	**Testing**: Finally, I tested the best-performing model on the test dataset.

### Proportion of value bets
How many value bets can the RF model identify? For the test data set, among all home bets, 58% were classified as value bets compared to 41% of all away bets were identified as value bets. Of the 58% home value bets, 52% were “correct” in the sense that the home team actually won, while 37% of away value bets were correct.

### Profitability: expected value
How about profitability? First, I'll calculate the *EV* of home and away value bets using the RF model estimated probabilities. We can interpret this as how profitable each singular bet is. One point I would like to mention is that I am not using the implied probabilities from the bookmakers' odds because I would find an EV of 0 for all home and away bets. Bookmakers set the EV intentionally to zero when they assign their odds.

- For home value bets, the average EV for a $10 wager was $6.85. Not so great because this is less than our initial wager amount. For away value bets, the average EV for a $10 wager was $10.44, which is pretty good -- we're at least getting a few cents more than our initial wager amount. 

- Let's compare this to a baseline strategy of just betting on the home team all the time, regardless of whether its a value bet or not. For all home bets, the average EV for a $10 wager was $-0.10 and for all away bets, the average EV for a $10 wager was $-1.63. Oof...

We can draw two conclusions from this. First, it seems that betting on away value bets is a better strategy than betting on home value bets. Second, using the RF model to identify what bets to make is better than blindly betting on all home or all away moneyline bets. 

### Profitability: return on investment
While EV measures the profitability of each individual bet, we can use *ROI* to evaluate long-term profitability. To calculate ROI, I'll divide the difference between the total payout and the total amount wagered by the total amount wagered. This measures the ROI as a percentage of the total amount wagered.

- For home value bets, the ROI for a $10 wager was -48% while for away value bets, the ROI was also -14%. Yikes! Even though you may earn a few extra cents betting on away value bets in terms of the EV for a single bet, the overall strategy of betting on away value bets is not profitable over the course of many bets.

- For all home and away bets, the ROI for a $10 wager was -98%. Double yikes!!

### Accuracy
In addition to profitability metrics, we can evaluate this model’s *accuracy*. I'll simply compare the number of times the RF model predicted a home team win and compare it to the number of times the home team actually won in the data. The RF model's accuracy was 89%, which is pretty accurate. We can also calculate the RF model's F1 score, which is a metric that is similar to accuracy but tells us how well a model balances precision and recall. Precision is the percentage of correctly predicted wins out of all predicted wins and recall is the percentage of correctly predicted wins out of all actual wins. I often get this confused so a helpful way to think about this to think about a fire alarm system. Precision is how many times did the fire alarm correctly go off when there was an actual fire? Recall is how many actual fires did the alarm detect? The RF model had a F1 score of 0.91 for predicting home team wins versus a 0.87 score for predicting away team wins (Note that F1 scores are not percentages but range between 0 and 1). So overall, the RF model does a decent job at predicting the binary win outcome.


### Here's a table that summarizes all this information.
<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; margin-bottom: 10px;">Test Data Set Evaluation Using RF</caption>
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
      <td style="border: 1px solid #ddd; padding: 8px;">0.4092</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.5181</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.3671</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">6.8492</td>
      <td style="border: 1px solid #ddd; padding: 8px;">10.4449</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.0981</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-1.6254</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-48.17</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-13.50</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-98.44</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-98.59</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Accuracy</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8917</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8917</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">F1 Score</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.9063</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8716</td>
    </tr>
  </tbody>
</table>

#### Model 2: Neural Collaborative Filtering 

The second machine learning model I tested was a Neural Collaborative Filtering (NCF) model inspired by recommendation systems models. Unlike a traditional neural network, this model was designed to incorporate the multi-dimensionality of the three sports in my data. My aim was for the model to learn relationships between teams across different sports, based on my hypothesis that there could be an underlying mechanism which explains a value bet in any sport.

<br>
The NCF model uses game features between teams (i.e. item features between users in recommendation systems) and maps them onto a higher-dimensional embedding space. This allows the model to learn complex, non-linear interactions between teams. The architecture includes four hidden layers: the first two are shared layers for all sports, while the last two are sport-specific layers. The final output layer uses a softmax activation function and is trained with a binary cross-entropy loss function. 

<br>
For hyperparameter tuning, I adjusted the dropout rate, learning rate, weight decay, and the number of embedding dimensions. I used a 5-fold cross validation method to evaluate performance, trained the model in batches of size 64, and ran 20 epochs. Once the model was optimized, I evaluated its performance on the validation set and then applied it to the test data. Finally, I implemented an ensemble approach, training 10 models and averaging their binary home team win predictions to obtain probabilities.

### Proportion of value bets
So how did this NCF model fare? First, the NCF model identified 50% of all home bets as home value bets compared to 40% of all away bets as away value bets. Out of the 50% home value bets, 44% were correct home value bets. Out of the 40% away value bets, 33% were correct away value bets. 

### Profitability
In terms of *EV*, for home value bets, the average EV for a $10 wager was $5.32 whereas for away value bets the average EV was $7.29. Not so great compared to the RF model. However, in terms of *ROI*, for home value bets the ROI was -38% while the ROI for away value bets was 0.26%! This is great news in the sense that the ROI for away value bets was finally positive!

### Accuracy
In terms of *accuracy*, the NCF model performed similarly to the RF model with an accuracy of 86%. The NCF model also had an F1 score of 0.88 for the home value bets and 0.83 for away value bets.

### The table below summarizes this information.
<table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; margin-bottom: 10px;">Test Data Set Evaluation Using NCF</caption>
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
      <td style="border: 1px solid #ddd; padding: 8px;">0.4995</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.3978</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Mean Correct Value Bet</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4354</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.3293</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Value Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">5.3235</td>
      <td style="border: 1px solid #ddd; padding: 8px;">7.2897</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Theoretical Average EV for Actual Bets</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.4746</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-0.4546</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Value Bet ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-37.78</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.26</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Actual ROI (%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-98.44</td>
      <td style="border: 1px solid #ddd; padding: 8px;">-98.59</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Accuracy</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8601</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8601</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">F1 Score</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8803</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.8317</td>
    </tr>
  </tbody>
</table>

# Conclusion

The goal of this project was to develop a ML model capable of evaluating moneyline sports bets to identify profitable bets. Depending on which profitability metric you prioritize, I found both the Random Forest and the Neural Collaborative Filtering model to be adequate. Though I may prefer the NCF model simply because it does better on ROI which means in the long-run you end up net positive (assuming you bet only on away moneyline bets). That gives me slightly more confidence in that model.

### Distribution of value bet payouts
An interesting observation from the results is that away value bets are more profitable than home value bets. I never would've predicted this, so to understand why, I decided to dig into the findings. First, I looked at the distribution of payouts for both correct home and away value bets (I'm examining correct value bets because these are the bets that pay out). I classified correct value bets into three categories: low (<33%), medium (33%<X< 67%), and high (>67%) probability of winning. The table below illustrates this distribution.

<br>
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
      <td style="border: 1px solid #ddd; padding: 8px;">33.44</td>
      <td style="border: 1px solid #ddd; padding: 8px;">7.26%</td>
      <td style="border: 1px solid #ddd; padding: 8px;">36.88</td>
      <td style="border: 1px solid #ddd; padding: 8px;">18.42%</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Medium (33-67%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">9.43</td>
      <td style="border: 1px solid #ddd; padding: 8px;">51.83%</td>
      <td style="border: 1px solid #ddd; padding: 8px;">10.66</td>
      <td style="border: 1px solid #ddd; padding: 8px;">59.98%</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">High (&gt;67%)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">2.98</td>
      <td style="border: 1px solid #ddd; padding: 8px;">40.91%</td>
      <td style="border: 1px solid #ddd; padding: 8px;">3.26</td>
      <td style="border: 1px solid #ddd; padding: 8px;">21.60%</td>
    </tr>
  </tbody>
</table>

<br>
You can see that the majority of correct away value bets (60%) are classified as having a medium probability of winning with an average payout of $10.66--slightly higher than the initial $10 wager. Compare this to correct home value bets in which the distribution is more evenly split among low, medium, and high probability of winning, and this may be insufficient to offset the losses from incorrect home value bets.

### Upsets and value bets
Additionally, if you think about the types of bets that earn the most money, it's usually when you correctly pick an upset. This could explain why away value bets are more profitable. There could be more upsets in which an underdog away team beats a favored home team compared to the other way around. Indeed, the data I found supports this: away teams upsetting home teams account for 45% of all upsets (you can see this in the [EDA section here](/pages/valuebets-part2)). Moreover, 46% of all *correct* away value bets were upsets (and 42% of all away value bets were upsets), compared to only 23% of all *correct* home value bets (and 26% of all home value bet being upsets). Since upsets offer larger payouts, it makes sense that away value bets are more profitable because they capture a greater share of these high payout events.

<br>
So did I achieve my goal? Yes, I think so. Hopefully this analysis also provided valuable insights about sports betting that you can use to make smarter wagers! If you want to test out my model in real time, you can click the link below. But use at your own risk!

<br>
[Use my model to help identify value bets!](https://value-bet-model-app.onrender.com/)

<br>
If you have any feedback, questions, or ideas, please reach out to me -- I'd love to discuss anything related to this project further!

### Thanks for reading!

