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

I'll outline my data preprocessing and features used for this project. 

#### Data Preprocessing

First I'll filter out all the games in which the outcome ended in a draw. I did this for two purposes: one, to make the modeling easier because I am evaluating only two outcomes, and two, not all sports in my data have draws an outcome. I'll also process some variables:

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

- **Win and Loss Streaks**:
    - I created four variables for every game that reset upon each new season:
        - 'home team win streak' and 'away team win streak': Binary variable that indicates if a team has won 3 consecutive games at home or away.
        - 'home team loss streak' and 'away team loss streak': Binary variable that indicates if a team has lost 3 consecutive games at home or away.
    - This feature should help me capture what is known as momentum or recent form in sports.
 
- **Upsets**:
    - An 'upset' occurs when the favored team (lower odds) loses so for example if the home team is favored (by the odds) but the away team wins.
    - I'll create two variables that track a historical upset percentage for each team when it appears as the home or away team.

- **Adjusted Historical Win Rates**:
    - I created four variables for every game:
        - 'home team win rate favored': Historical win rate of the home team when it was favored
        - 'home team win rate underdog': Historical win rate of the home team when it was the underdog
        - 'away team win rate favored'
        - 'away team win rate underdog'
    - These variables help establish a baseline win rate for every team conditional on whether that team was favored to win.

- **Elo Ratings**:
    - I created two variables for every game that reset upon each new season:
    - 'home team elo'
    - 'away team elo'
    - Elo rating is calculated using a formula from this [website](https://www.aussportstipping.com/sports/nfl/elo_ratings/) to rank teams. For more info about Elo ratings for different sports, you can check out these links: [World Football Elo](https://www.eloratings.net/about), [NBA Elo](https://fivethirtyeight.com/features/how-we-calculate-nba-elo-ratings/#:~:text=Take%20a%20team's%20margin%20of,accounting%20for%20home%2Dcourt%20advantage), and [NFL Elo](https://fivethirtyeight.com/features/introducing-nfl-elo-ratings/).
    - These variables help provide a dynamic measure of team strength.

- **Elo Ratings Difference**:
    - This variable finds the difference between the home team elo rating and the away team elo rating.
    - Sometimes the home and away odds may not reflect "power rankings" in sports. Elo ratings in theory do a better job at describing how "strong" a team is at the moment so this feature should help quantify the difference in strength between the two teams.

- **Elo Ratings Discrepancy**:
    - Based on the Elo ratings, I created this three-level variable to categorize differences in elo ratings from a categorical perspective. If Elo ratings differ by more than 50 points, the variable classifies this discrepancy as large positive or negative. If Elo ratings differ by less than 50 points, then this variable classifies this discrepancy as small.

- **Home Odds and Elo Mismatch**:
    - This variable assigns a 1 if there is a difference in whether the home team was favored by the odds or the Elo ratings and 0 if there is no mismatch. For instance, if the home team is favored by the odds (the home odds < away odds) but the home team is not favored by the Elo ratings (the home elo rating > the away elo rating), then this variable is assigned a 1 because there is a mismatch.

- **Away Odds and Elo Mismatch**:
    - This variable assigns a 1 if there is a difference in whether the away team was favored by the odds or the Elo ratings and 0 if there is no mismatch. For instance, if the away team is favored by the odds (the home odds > away odds) but the away team is not favored by the Elo ratings (the home elo rating < the away elo rating), then this variable is assigned a 1 because there is a mismatch.
      
<br>

[Return to Part 1](/projects/valuebets)
<br>
[Go to Exploratory Data Analysis](/pages/valuebets-part2)
<br>
[Go to Part 2](/pages/valuebets-part3)