---
layout: page
title: Using ML to Identify Value Bets
permalink: /projects/valuebets
---

## Identifying Value Bets in Sports Using Machine Learning

[Methodology](/pages/valuebets-method)
<br>
[Exploratory Data Analysis](/pages/valuebets-part2)
<br>
[Skip to Part 2](/pages/valuebets-part3)
<br>
[Use my model to help identify value bets!](https://value-bet-model-app.onrender.com/)

<br><br>
*How do I earn a profit in sports betting?*  
<br>
 
A quick Google search reveals 2 common sports betting strategies: **arbitrage betting** or **value betting**. Arbitrage betting involves placing multiple bets on the same event, often using different bookmakers, to guarantee a positive return. This works because bookmakers typically set different odds for the same bets. Value betting on the other hand is a strategy where bettors identify bets they believe have a higher probability of occuring than what the bookmaker believes when setting the odds. 

### Value betting

Between the two, value betting seems more intuitive to me for two reasons. One, I'm too lazy to open different betting apps to place arbitrage bets. Two, I often evaluate betting events such as whether Lebron James will score more than 15+ points by judging whether the odds of this event reflect how likely I think that event will occur.

<br>

But the process of judging the "true" odds of an event occuring feels very subjective in the context of sports betting. Unlike a coin flip, where the true probability of landing heads is objectively 50% (or American odds of +100), there isn’t an easily obtainable “ground truth” for an event like whether LeBron James will score more than 15 points in a game. So then how do we determine the "true" odds? 

<br>

For starters, a simple and hypothetical method might be to survey a thousand sports fans and ask their opinion about the probability of an event occuring. This idea leverages the [wisdom-of-the-crowd effect](https://en.wikipedia.org/wiki/Wisdom_of_the_crowd) and seems like a pretty solid approach. However, it's not very feasible to collect a thousand responses for each betting event as that would take too much time and money. 

### Procedure

Here is where machine learning (ML) comes into play. I could train a model on sports betting events, using data such as bookmaker odds and actual outcomes, to predict probabilities of teams winning or losing. I could then use these model-predicted probabilities to identify value bets by comparing them to the bookmaker’s implied probabilities (derived from the odds). Then I can evaluate whether betting on these value bets leads to profit based on expected value (EV) and return on investment (ROI).

# Data

So how am I going to train a ML model? First, I need data on historical sports bets. I collected historical sports bets from the website [Oddsportal](https://www.oddsportal.com/) for three sports: American football, basketball, and soccer. 

- American football (National Football League): 11 seasons of data were collected, starting from the 2013-2014 season.
- Soccer (English Premier League): 11 seasons of data were collected, also beginning from the 2013-2014 season.
- Basketball (National Basketball Association): 9 seasons of data were collected, starting from the 2014-2015 season. 

Each entry in the dataset corresponds to a single bookmaker's odds for a game. For every game, the data set includes the following information:

- Game details: Home team, away team, game date, home score and away score.
- Bookmaker Odds: Home team and away team odds as well as draw odds (soccer only) for multiple bookmakers
- Game Odds: Average home team and away team odds; highest home team and away team odds
- Game ID: a unique identifier for each game

The code I used to collect the data can be found on my github [here](https://github.com/jeffreylckang/OPscraper.git).
The code for this project can be accessed on my github [here](https://github.com/jeffreylckang/valuebets.git).  
<br>

I'll skip the methodology section because it is quite lengthy. If you're curious about it, you can check it out [here](/pages/valuebets-method).
I'll also skip the exploratory data analysis because that too can be quite long to read. If you want to see some cool plots and learn about the data, click [here](/pages/valuebets-part2).

# Procedure

To identify value bets, I will compare the following models:

1. **Random Forest Model**: A random forest model will be trained on data from all sports to see if it can better identify value bets that result in improved betting performance compared to the baseline.

2. **Neural Collaborative Filtering (NCF) Model**: I will train an NCF model with four hidden layers (two shared sport layers and two sport-specific layers) and one output layer using data from all sports. I'm using a collaborative filtering model because I'm hoping to use the multi-sport dimensionality to leverage cross-sport patterns. My genius thought process is that there should be some underlying mechanisms that occur across different sports that can be used to help evaluate what makes a value bet for moneyline bets.

<br>
To evaluate betting performance, I will compare each model using two key metrics:

1. **Theoretical Expected Value (EV)**: This assesses the theoretical expected return on each incremental value bet, telling us in the short run how profitable strategies are. The reason this is theoretical is because the formula uses the model implied probability of winning and not the actual probability of winning set by the bookmaker.
   
    <br>
    $$\text{EV} = (\text{Probability of Win} \times \text{Profit}) - (\text{Probability of Loss} \times \text{Bet Amount})$$
    
    <br>

2. **Return on Investment (ROI)**: This metric will evaluate the total returns over time, telling us in the long run how profitable strategies are.
   
    <br>
    $$\text{ROI} = \frac{\text{Net Profit}}{\text{Total Bet Amount}} \times 100$$

<br>
Additionally, I will benchmark each model's EV and ROI against a baseline "dummy" strategy of always betting on the home or away team each time.  
<br>

[Continue to Part 2](/pages/valuebets-part3)  
<br><br>
