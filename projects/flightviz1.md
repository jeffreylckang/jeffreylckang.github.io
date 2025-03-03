---
layout: page
title: Visualizing Flight Prices
permalink: /projects/flightviz
---

## Visualizing Flight Ticket Prices from NYC and London

### How do flight ticket prices fluctuate over time?

I've always been curious about finding the optimal time to purchase flight tickets, so I decided to collect some data on it. Below, you will be able to find different visualizations that reveal some interesting results!

Read about the data collection using [Amadeus' Flight Offers API](https://developers.amadeus.com/self-service/category/flights/api-doc/flight-offers-search) [here](/pages/flightvizdata).

# 
### Research Question: How do flight prices fluctuate over time?
1. Are there specific number dates (out of 30) when flight tickets are cheaper or more expensive? compare to all data average. Maybe use a calendar as the viz 
2. Are there specific days of the week when flight tickets are cheaper or more expensive? compare to all data average. Can use week calendar as the viz.
3. Heatmap to combine both: Days of week as rows and Month as Col and so each box is going to be average of all Mondays for that Month
4. Airlines bar graph (average across all data). Airlines bar graph differential to all data average.
5. How far booking in advance guarentee lower prices? AKA whats the optimal booking window? 30 days, 60 days, 90 days, 120 days in advance? X is the days in advance and Y is the average flight price using heatmap
6. Most exp vs cheap differential (so largest differential) for same departure date -> average of this plotted across days to departure, day of week, day of month?
7. Heatmap showing price variance across booking days: X axis is days before departure, Y is departure days and color is price variance.

Possible to make the size of the text represent the N for the denom for average?
