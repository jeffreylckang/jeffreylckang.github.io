---
layout: page
title: Using ML to Identify Value Bets
permalink: /projects/valuebets
---

## Identifying Value Bets in Sports Using Machine Learning
[Skip to Part 2](/pages/valuebets-part2)
<br>
[Skip to Part 3](/pages/valuebets-part3)

<br><br>
*How can I make money in sports betting?*  
<br>

If you're like me, you probably asked yourself this question after losing bet after bet after bet. *It must be because I'm making the wrong choices.* I need to figure out how to make the right choices so that I win. Easy enough, right?

<br>
Let's formalize this. My objective is to earn a profit in sports betting. A quick Google search revealed 2 main strategies to achieve this outcome: **arbitrage betting** or **value betting**. Arbitrage betting involves placing multiple bets on the same event, often using different bookmakers, to guarantee a positive return. This works because bookmakers typically set different odds for the same bets. Value betting on the other hand is a strategy where bettors identify bets they believe have a higher probability of occuring than what the bookmaker believes when setting the odds.  
<br>
Between the two, value betting immediately seemed more intuitive to me for two reasons. One, I'm too lazy to open different betting apps to place arbitrage bets. Two, I often evaluate betting events such as whether Lebron James will score more than 15+ points by judging whether the odds of this event reflect how likely I think that event will occur.  
<br>
Before we go through an example, let me provide a quick refresher on odds for anyone unfamiliar with the domain. In this project, I'm going to be using the American odds format, which represents odds with + or - sign to indicate the direction of how likely that event is to occur. The plus sign indicates an unlikely event whereas the minus sign indicates a likely event. It may be more intuitive (at least to me) to think of this using probability, so the odds of -200 suggests a 67% chance of occuring. The formula to convert American odds to probability is:  

<br>
For negative odds:
$$\text{Probability} = \frac{|\text{Odds}|}{|\text{Odds}| + 100}$$

For positive odds:
$$\text{Probability} = \frac{100}{\text{Odds} + 100}$$  

<br>
To find profit, all you have to do is follow this formula below. Say for an odds of -200, if you place a $10 bet, you would receive $5 for a total payout of $15.  
<br>

Profit for negative odds:
$$\text{Profit} = \frac{\text{Bet Amount}}{|\text{Odds}|} \times 100$$

Profit for positive odds:
$$\text{Profit} = \frac{\text{Bet Amount} \times \text{Odds}}{100}$$  

<br>
Now that we understand odds, let's go through an example. If the odds for Lebron James scoring more than 15+ points is listed as -200, but I believe the odds should be closer to -300, then I see this as a "good" bet. This is what is referred to as a **value bet**: a situation in which I believe the "true" odds are lower than -200, or the probability for this event to occur is higher than 67%. 

# Motivation

One of the challenges I quickly noticed with identifying value bets in sports is how subjective this process feels. Unlike a coin flip, where the true probability of landing heads is objectively 50% (or American odds of +100), there isn’t an easily obtainable “ground truth” for an event like whether LeBron James will score more than 15 points in a game. Even so, maybe I could brainstorm a way to arrive closer to the "ground truth" without knowing exactly what it is. All I have to do is reliably identify value bets in an objective way and then I could theoretically earn a profit.  

<br>
For starters, a simple and hypothetical method might be to survey a thousand sports fans and ask their opinion about the probability of an event occuring. This idea leverages the [wisdom-of-the-crowd effect](https://en.wikipedia.org/wiki/Wisdom_of_the_crowd) and seems like a pretty solid approach. However, it's not very feasible to collect a thousand responses for each betting event as that would take too much time and money. Here is where machine learning (ML) comes into play. I could train a model on sports betting events, using data such as bookmaker odds and actual outcomes, to predict probabilities of teams winning or losing. I could then use these model-predicted probabilities to identify value bets by comparing them to the bookmaker’s implied probabilities (derived from the odds).  

<br>
Coincidentally, I happened to be discussing the moneyline odds (the sports betting jargon for wagering on a team's outright win) of the Jets vs. the 49ers game with a friend (shoutout Brian). 

<br>
*Me: There's no way the Jets are going to beat the 49ers*.

<br>
*Friend: "Really? Well, what do you think is more likely? The Jets beating the 49ers or Tottenham uspetting Manchester City this weekend?"* 

<br>
Keep in mind that the moneyline odds for the Jets winning were +162, while the odds for Tottenham upsetting Manchester City were +475. At that moment, his question sparked a realization. Perhaps a model leveraging data from different sports where similar “upset” scenarios occur might enhance a model’s ability to evaluate value bets because the model can compare upsets in different sports. Conceptually, you could think of this like gaining additional perspectives to improve decision-making, similar to collecting a "wisdom of the crowd" for betting scenarios.  

<br>
>Thus, the plan for this project is to use ML to identify value bets and test its effectiveness as a betting strategy. Specifically, I aim to compare two models: one trained on data from multiple sports and another trained on data from a single sport. The key question is whether including multi-sport data improves the model’s performance on profitability metrics like expected value (EV) and return on investment (ROI). 

# Data

Before describing the methodology, let me first provide some context about the data. I collected data from the website [Oddsportal](https://www.oddsportal.com/). The dataset spans three sports: American football, basketball, and soccer. 

- American football (National Football League): 11 seasons of data were collected, starting from the 2013-2014 season.
- Soccer (English Premier League): 11 seasons of data were collected, also beginning from the 2013-2014 season.
- Basketball (National Basketball Association): 9 seasons of data were collected, starting from the 2014-2015 season. 

Each entry in the dataset corresponds to a single bookmaker's odds for a game. For every game, the data set includes the following information:

- Game details: Home team, away team, game date, home score and away score.
- Bookmaker Odds: Home team and away team odds as well as draw odds (soccer only) for multiple bookmakers
- Game Odds: Average home team and away team odds; highest home team and away team odds
- Game ID: a unique identifier for each game

The code I used to collect the data can be found on my github [here](https://github.com/jeffreylckang/OPscraper.git).

# Methodology

In this section, I'll outline my data preprocessing, feature engineering and procedure used for this project. 

#### Data Preprocessing

First I'll filter out all the games in which the outcome ended in a draw. I did this for two purposes: one, to make the modeling easier because I am evaluating only two outcomes, and two, not all sports have draws an outcome. I'll also process some variables:

- **Home Team Win**:
    - I calculated the outcome of each game by comparing the home and away scores. 
    - A win for the home team is denoted as 1, a win for the away team is denoted as 0, and draws are NaNs.
          
- **Season**:
    - A 'season' variable was added to track which season each game belongs to, based on the date of the game.

#### Feature Engineering

- **Odds Ratios**:
    - 'average odds ratio': Ratio of the average home odds to away odds.
    -  However, the American odds format makes the odds ratio not interpretable, so I will adjust this ratio by taking the absolute value of both the home and away odds if either of the odds' signs are mixed. If the odds have the same sign, then I will directly take the odds ratio. This should make the odds ratio interpretable.
    - This feature should capture how bookmakers value the relative strengths of the home and away team for each game.

- **Odds Deviations**:
    - I created two variables for every game:
        - 'average deviation of home odds': Deviation between each bookmaker's home odds and the average home odds provided by Oddsportal
        - 'average deviation of away odds'
    - This feature should help quantify the variance in odds across bookmakers for each game.

- **Win and Loss Streaks**:
    - I created four variables for every game that reset upon each new season:
        - 'home team win streak' and 'away team win streak': Binary variable that indicates if a team has won 3 consecutive games at home or away.
        - 'home team loss streak' and 'away team loss streak': Binary variable that indicates if a team has lost 3 consecutive games at home or away.
    - This feature should help me capture what is known as momentum or recent form in sports.
 
- **Upsets**:
    - An 'upset' occurs when the favored team (lower odds) loses so for example if the home team is favored but the away team wins.
    - This feature is a binary variable for 1 if the underdog team wins and 0 if the favored team wins.
 
- **Rolling Score Differential**:
    - I created two variables for every game that reset upon each new season:
        - 'rolling average home differential': Averages the last 5 score differentials for the team when it is at home.
        - 'rolling average away differential'
    - The score differentials are standardized by sport to account for the fact that the three sports have different scoring magnitudes.
    - This feature should help quantify team performance similar to momentum or recent form but in a numerical form.

- **Adjusted Historical Win Rates**:
    - I created two variables for every game:
        - 'adjusted win rate home': Historical win rate of the home team weighted by the proportion of games they are favored or are underdogs.
        - 'adjusted win rate away'
    - These variables help establish a baseline win rate for every team conditional on whether that team was favored to win.

- **Elo Ratings**:
    - I created two variables for every game that reset upon each new season:
    - 'home team elo'
    - 'away team elo'
    - Elo rating is calculated using a formula to rank teams based on [World Football Elo](https://www.eloratings.net/about), [NBA Elo](https://fivethirtyeight.com/features/how-we-calculate-nba-elo-ratings/#:~:text=Take%20a%20team's%20margin%20of,accounting%20for%20home%2Dcourt%20advantage), and [NFL Elo](https://fivethirtyeight.com/features/introducing-nfl-elo-ratings/).
        - An important detail is I adjusted the K-factor for the formula (K basically tells you how much weight to give to recent games/performances) based on different sports.
        - Basketball: $$k + (0.2 \times \max(0, (\text{pd} - 5) // 10))$$
        - Football: $$k \times (0.75 + (\text{pd} - 3) / 8)$$  
        - American Football: $$k + (0.25 \times \max(0, (\text{pd} - 7) // 7))$$
    - These variables help provide a dynamic measure of team strength.

- **Cross-Sport Elo Similarity**:
    - Based on the home and away team elo ratings, I created two variables:
        - 'similar home team elo': Average of the top two most similar elo rated home teams.
        - 'similar away team elo'
            - Similarities were first calculated using cosine similarity.
        - These features leverage cross-sport data for evaluating matchups.

#### Procedure

To identify value bets, I will compare the following models:

1. **Historical Win Rate Analysis**: I will first analyze the historical win rate of teams, conditional on the odds favoring the team, to establish a baseline model for identifying value bets.

2. **Random Forest Model**: A random forest model will be trained on data from all sports to see if it can better identify value bets that result in improved betting performance compared to the baseline.

3. **Neural Network (NN) models**:
    - **Feedforward NN**: I will train a simple feedforward NN with three hidden layers and one output layer. This model will be trained exclusively on a single sport data (basketball).
    - **Neural Collaborative Filtering (NCF) Model**: I will train an NCF model with three hidden layers and one output layer using data from all sports. The collaborative filtering component leverages embeddings of teams and sports data to identify cross-sport patterns, potentially improving the ability to evaluate value bets.

<br>
To evaluate betting performance, I will compare each model using two key metrics:

1. **Theoretical Expected Value (EV)**: This assesses the theoretical expected return on each incremental value bet, telling us in the short run how profitable strategies are. The reason this is theoretical is because the formula uses the model implied probability of winning and not the actual probability of winning set by the bookmaker.
   
    <br>
    $$\text{EV} = (\text{Probability of Win} \times \text{Profit}) - (\text{Probability of Loss} \times \text{Bet Amount})$$

2. **Return on Investment (ROI)**: This metric will evaluate the total returns over time, telling us in the long run how profitable strategies are.
   
    <br>
    $$\text{ROI} = \frac{\text{Net Profit}}{\text{Total Bet Amount}} \times 100$$

<br>
Additionally, I will benchmark each model's EV and ROI against a baseline "dummy" strategy of consistently betting on the home or away team.  

<br>
The data will be split as follows:
- 80% Training
- 10% Validation
- 10% Test  

In addition, I will hold out the 2022-2023 NBA season as a separate test set to evaluate the models' generalizability on completely unseen data.  

<br>
The code I used for the analysis can be found on my github [here](https://github.com/jeffreylckang/valuebets.git).  
<br>

---
Awesome! Next we'll look at some descriptive statistics and explore the data in detail.

[Continue to Part 2](/pages/valuebets-part2)  
<br><br>
