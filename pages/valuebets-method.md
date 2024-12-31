---
layout: page
title: Using ML to Identify Value Bets Methodology
permalink: /pages/valuebets-method
---

## Identifying Value Bets in Sports Using Machine Learning: Methodology

[Return to Part 1](/projects/valuebets)
<br>
[Go to Exploratory Data Analysis](/pages/valuebets-part2)
<br>
[Go to Part 2](/pages/valuebets-part3)

# Methodology

I'll outline my data preprocessing, feature engineering and procedure used for this project. 

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
<br>
---

[Return to Part 1](/projects/valuebets)
<br>
[Go to Exploratory Data Analysis](/pages/valuebets-part2)
<br>
[Go to Part 2](/pages/valuebets-part3)