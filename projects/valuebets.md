---
layout: page
title: Using ML to Identify Value Bets
permalink: /projects/valuebets
---

## Identifying Value Bets in Sports Using Machine Learning
[Part 2](pages/valuebets-part2.md)

*How can I make money in sports betting?* 

If you're like me, you probably asked yourself this question after confidently placing what felt like a sure bet--only to end up emotionally damaged at the end of the game. Throughout my entire amateur sports betting career, I can definitively say that I'm net negative. While that isn't ideal, it's something I'm okay with because betting for me has always been about having fun. But recently I've started wondering: what if I actually tried to approach sports betting more seriously? Is there some strategy or method I could use to identify what I think could be considered "good" bets?  

So I thought about sports betting and this project is a reflection of my thought process on how to potentially be net positive. But before I dive into the details, let me start off by saying that I'm not an expert in this domain. Furthermore, the findings of this project are not intended as advice or recommendations. This project is for my amusement and an opportunity to see if I can apply some of the critical thinking skills I developed during my PhD to a topic I'm passionate about. 

### Here's a short abstract about the results and what I learned

Write abstract

# Introduction

Now it's clear what my objective is: to earn a profit in sports betting. A quick Google search revealed 2 main strategies to achieve this outcome: **arbitrage betting** or **value betting**. Arbitrage betting involves placing multiple bets on the same event, often using different bookmakers, to guarantee a positive return. This works because bookmakers typically set different odds for the same bets. Value betting on the other hand is a strategy where bettors identify bets they believe have a higher probability of occuring than what the bookmaker believes when setting the odds.  

Value betting immediately seemed more intuitive to me--I often evaluate betting events such as whether Lebron James will score more than 15+ points by judging whether the odds of this event reflect how likely I think the event will occur.  

Before we go through an example, let me provide a quick refresher on odds. In this project, I'm going to be using the American odds format, which represents odds with + or - sign to indicate the direction of how likely that event is to occur. The plus sign indicates an unlikely event whereas the minus sign indicates a likely event. It may be more intuitive (at least to me) to think of this using probability, so the odds of -200 suggests a 67% chance of occuring. The formula to convert American odds to probability is:  

For negative odds:
$$\text{Probability} = \frac{|\text{Odds}|}{|\text{Odds}| + 100}$$

For positive odds:
$$\text{Probability} = \frac{100}{\text{Odds} + 100}$$
<br>
Now for an odds of -200, if you place a $10 bet, you would receive $5 for a total payout of $15. 

Profit for negative odds:
$$\text{Profit} = \frac{\text{Bet Amount}}{|\text{Odds}|} \times 100$$

Profit for positive odds:
$$\text{Profit} = \frac{\text{Bet Amount} \times \text{Odds}}{100}$$
<br>
Okay back to my example. If the odds for Lebron James scoring more than 15+ points is listed as -200, but I believe the odds should be closer to -300, then I see this as a "good" bet. This is what is referred to as a value bet: a situation in which I believe the "true" odds are higher than -200, or the probability for this event to occur is higher than 67%. 

# Motivation

One of the challenges I quickly noticed with identifying value bets in sports is how subjective this process feels. Unlike a coin flip, where the true probability of landing heads is objectively 50% (or American odds of +100), there isn’t an easily obtainable “ground truth” for an event like whether LeBron James will score more than 15 points in a game. Despite this, maybe I could brainstorm a way to arrive closer to the "ground truth" without knowing exactly what it is. All I have to do is reliably identify value bets with more than just my opinion and then I could theoretically turn a profit. 
<br>
For starters, a simple and hypothetical method might be to survey a thousand sports fans and ask their opinion about the probability of an event occuring. This idea leverages the [wisdom-of-the-crowd effect](https://en.wikipedia.org/wiki/Wisdom_of_the_crowd) and is a pretty solid approach; however, it's not feasible as collecitng a thousand responses doesn't seem realistic. Here is where machine learning (ML) comes into play. I could train a model on sports betting events, using data such as bookmaker odds and actual outcomes, to predict probabilities of teams winning or losing. I could then use these model-predicted probabilities to identify value bets by comparing them to the bookmaker’s implied probabilities (derived from the odds).
<br>
Coincidentally, I happened to be discussing the moneyline odds (the sports betting jargon for wagering on a team's outright win) of a specific game with a friend: the Jets vs. the 49ers. I mentioned that I didn't think the Jets were likely to beat the 49ers. He then replied, *"Well, what do you think is more likely? The Jets beating the 49ers or Tottenham uspetting Manchester City this weekend?"* The moneyline odds for the Jets winning were +162, while the odds for Tottenham upsetting Manchester City were +475. At that moment, his question sparked a realization. Perhaps a model leveraging data from different sports where similar “upset” scenarios occur might enhance a model’s ability to evaluate events like the Jets upsetting the 49ers. Conceptually, you could think of it like gaining additional perspectives to improve decision-making, similar to collecting a "wisdom of the crowd" for betting scenarios.
<br>
>Thus, the plan for this project is to use ML to identify value bets and test its effectiveness as a betting strategy. Specifically, I aim to compare two models: one trained on data from multiple sports and another trained on data from a single sport. The key question is whether including multi-sport data improves the model’s performance on metrics like expected value (EV) and return on investment (ROI). 

# Data

Before describing the methodology, let me first provide some context about the data. I collected data from the website [Oddsportal](https://www.oddsportal.com/). The dataset spans three sports: American football, basketball, and soccer. 

- American football (National Football League): 11 seasons of data were collected, starting from the 2013-2014 season.
- Soccer (English Premier League): 11 seasons of data were collected, also beginning from the 2013-2014 season.
- Basketball (National Basketball Association): 9 seasons of data were collected, starting from the 2014-2015 season. 

<br>
Each entry in the dataset corresponds to a single bookmaker's odds for a game. For every game, the data set includes the following information:

- Game details: Home team, away team, game date, home score and away score.
- Bookmaker Odds: Home team and away team odds as well as draw odds (soccer only) for multiple bookmakers
- Game Odds: Average home team and away team odds; highest home team and away team odds
- Game ID: a unique identifier for each game

<br>
The code I used to collect the data can be found on my github [here](https://github.com/jeffreylckang/OPscraper.git).

# Methodology

In this section, I'll outline my data preprocessing, feature engineering and procedure used for this project. 

#### Data Preprocessing

First I'll filter out all the games in which the outcome ended in a draw. I'll also process some variables:

- **Home Team Win**:
    - I calculated the outcome of each game by comparing the home and away scores. 
    - A win for the home team is denoted as 1, a win for the away team is denoted as 0, and draws are NaNs.
    - For this project, I excluded games that ended in a draw because I wanted to focus on bets related to the home or away team wins.
    
- **Season**:
    - A 'season' variable was added to track which season each game belongs to, based on the date of the game.

#### Feature Engineering

- **Odds Ratios**:
    - I created two variables for every game:
        - 'average odds ratio': Ratio of the average home odds to away odds.
        - 'highest odds ratio': Ratio of the highest home odds to away odds.
    - This should capture how bookmakers value the relative strengths of the home and away team for each game.

- **Odds Deviations**:
    - I created two variables for every game:
        - 'average deviation of home odds': Deviation between each bookmaker's home odds and the average home odds provided by Oddsportal
        - 'average deviation of away odds': Same as above but for the away odds
    - This should help quantify the variance in odds across bookmakers for each game.

- **Win and Loss Streaks**:
    - I created four variables for every game that reset upon each new season:
        - 'home team win streak' and 'away team win streak': Binary variable that indicates if a team has won 3 consecutive games at home or away.
        - 'home team loss streak' and 'away team loss streak': Binary variable that indicates if a team has lost 3 consecutive games at home or away.
    - This should help me capture what is known as momentum or recent form in sports.
 
- **Upsets**:
    - An 'upset' occurs when the favored team (lower odds) loses so for example if the home team is favored but the away team wins.
    - This is a binary variable for 1 if the underdog team wins and 0 if the favored team wins.
 
- **Rolling Score Differential**:
    - I created two variables for every game that resets every season:
        - 'rolling average home differential': Averages the last 5 score differentials for the team when it is at home.
        - 'rolling average away differential': Same as above but for the away team.
    - This should help quantify team performance similar to momentum or recent form but numerically.

- **Adjusted Historical Win Rates**:
    - I created two variables for every game
        - 'adjusted win rate home': Historical win rate of the home team weighted by the proportion of games they are favored or are underdogs.
        - 'adjusted win rate away': Same as above but for the away team.
    - These variables help establish a baseline win rate for every team conditional on whether that team was favored to win.

- **Elo Ratings**:
    - I created two variables for every game which resets every season:
    - 'home team elo'
    - 'away team elo'
    - Elo rating is calculated using a formula to rank teams based on [World Football Elo](https://www.eloratings.net/about), [NBA Elo](https://fivethirtyeight.com/features/how-we-calculate-nba-elo-ratings/#:~:text=Take%20a%20team's%20margin%20of,accounting%20for%20home%2Dcourt%20advantage), [NFL Elo](https://fivethirtyeight.com/features/introducing-nfl-elo-ratings/)
        - An important detail is I adjusted the K-factor for the formula (K basically tells you how much weight to give to recent games/performances) based on different sports
        - Basketball: \( k + (0.2 \times \max(0, (\text{pd} - 5) // 10)) \)  
        - Football: \( k \times (0.75 + (\text{pd} - 3) / 8) \)  
        - American Football: \( k + (0.25 \times \max(0, (\text{pd} - 7) // 7)) \)  
    - These variables help provide a dynamic measure of team strength.

- **Cross-Sport Elo Similarity**:
    - Based on the home and away team elo ratings, I created two variables:
        - 'similar home team elo': Average of the top two most similar elo rated home teams
        - 'similar away team elo': Average of the top two most similar elo rated away teams
            - Similarities were first calculated using cosine similarity
        - This leverages cross-sport data for evaluating matchups.

#### Procedure

To identify value bets, I will compare the following models:

1. **Historical Win Rate Analysis**: I will first analyze the historical win rate of teams, conditional on the odds favoring the team, to establish a baseline model for identifying value bets.

2. **Random Forest Model**: A random forest model will be trained to see if it can better identify value bets that result in improved betting performance compared to the baseline.

3. **Neural Network (NN) models**:
    - **Feedforward NN**: I will train a simple feedforward NN with three hidden layers and one output layer. This model will be trained exclusively on basketball data to examine performance for a single sport.
    - **Neural Collaborative Filtering (NCF) Model**: I will train an NCF model with three hidden layers and one output layer using data from all sports. The collaborative filtering component leverages embeddings of teams and sports data to identify cross-sport patterns, potentially improving the ability to evaluate value bets.
<br>
To evaluate betting performance, I will compare each model using two key metrics:

1. **Expected Value (EV)**: This assesses the expected return on each incremental value bet, telling us in the short run how profitable strategies are.
$$
\text{EV} = (\text{Probability of Win} \times \text{Profit}) - (\text{Probability of Loss} \times \text{Bet Amount})
$$
3. **Return on Investment (ROI)**: This metric will evaluate the total returns over time, telling us in the long run how profitable strategies are.
$$
\text{ROI} = \frac{\text{Net Profit}}{\text{Total Bet Amount}} \times 100
$$
<br>
Additionally, I will benchmark each model's EV and ROI against a baseline "dummy" strategy of consistently betting on the home or away team.
<br>
The data will be split as follows:
- 80% Training
- 10% Validation
- 10% Test
<br>
In addition, I will hold out the 2022-2023 NBA season as a separate test set to evaluate the models' generalizability on completely unseen data.

The code I used for the analysis can be found on my github [here](https://github.com/jeffreylckang/valuebets.git).


Awesome! Next we'll look at some descriptive statistics and explore the data in detail.

[Continue to Part 2](pages/valuebets-part2.md)
