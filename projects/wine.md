---
layout: page
title: Forecasting Wine Vintage Scores
permalink: /projects/wine
---

## Forecasting and Analyzing Wine Vintage Scores

[Go to Part 2 Forecasting](/pages/wine-part2)
<br>
[Go to Part 3 Important Features](/pages/wine-part3)

<br>

Drinking wine is one of my favorite hobbies, and I genuinely get excited each time I have a glass as I try to figure out how 'good' a wine tastes. It's fun to compare my own subjective ratings with more 'objective' vintage scores. Different organizations hand these out, and all of them in some sense score a wine based on the year it was made. 

<br>

Now I don't know too much about how these vintages are scored, but what I found really interesting to think about is what  makes a vintage 'good' or 'bad' in the first place? I've heard people throw around comments before like "2020 was a great year because the weather was favorable". But what does that actually mean? Unless you're a seasoned grape grower, it's hard to understand what contributes to this vintage score.

### Does weather predict wine quality?

# Outline

So my idea behind this project is to try to connect the dots and see if actual weather data can tell us something about why certain years get higher vintage scores. I'll approach this in 2 ways. First, I'll try to see if I could build a model to predict wine scores based on historical weather data. Second, I'll analyze the historical weather data and identify what factors contribute the most to wine vintage scores.

# Data

I collected wine vintage scores from [Wine Enthusiast](https://www.wineenthusiast.com/wine-vintage-chart/). The vintage score data is theoretically scored from 0 to 100. However, in the data the scores range from 81 to 100. The data covers years from 1997 to 2025 and I targeted popular wine producing countries, noting the grape/wine type by each region within that country (i.e. some regions within a country have more than one wine type). One limitation of this data is that in reality, many regions produce more than 1-3 types of wine. For instance, Napa Valley produces Syrah which is not listed on Wine Enthusiast. This means that these unlisted wines are out of the scope of this project. In the end, the dataset contained 16 regions spanning across U.S., Italy, Australia, Argentina, Chile, and France for a total of 26 unique wine-region combinations. 

<br>

The table below shows some descriptive statistics of the wine **vintage scores**.

<br>

<table style="border-collapse: collapse; width: 50%; margin-left: auto; margin-right: auto;">
  <tr style="border: 1px solid black;">
    <th style="border: 1px solid black; text-align: center;">Statistic</th>
    <th style="border: 1px solid black; text-align: center;">Value</th>
  </tr>
  <tr style="border: 1px solid black;">
    <td style="border: 1px solid black; text-align: center;">Mean</td>
    <td style="border: 1px solid black; text-align: center;">91.67</td>
  </tr>
  <tr style="border: 1px solid black;">
    <td style="border: 1px solid black; text-align: center;">Standard Deviation</td>
    <td style="border: 1px solid black; text-align: center;">3.27</td>
  </tr>
  <tr style="border: 1px solid black;">
    <td style="border: 1px solid black; text-align: center;">Median</td>
    <td style="border: 1px solid black; text-align: center;">92.00</td>
  </tr>
  <tr style="border: 1px solid black;">
    <td style="border: 1px solid black; text-align: center;">Minimum</td>
    <td style="border: 1px solid black; text-align: center;">81.00</td>
  </tr>
  <tr style="border: 1px solid black;">
    <td style="border: 1px solid black; text-align: center;">Maximum</td>
    <td style="border: 1px solid black; text-align: center;">100.00</td>
  </tr>
</table>

<br>

Correpsonding historical weather data for the same 16 regions was sourced from [WeatherSpark](https://weatherspark.com/). The site requires a paid subscription, so to download the data, I paid for a month (sad). One major limitation to note of this data is that the historical data comes from the nearest airport meteorological station. This means that the weather data location is approximate to the actual vineyard areas, so the available data reflects general weather conditions across a broader region, not the specific microclimates of individual vineyard plots. Because of this, I assume that the regional weather can serve as a proxy for the conditions affecting all listed wine types in that region. 

<br>

The historical weather data contains observations at the daily level, but for the purposes of my analysis, I'll be aggregating the data to the monthly level. To handle missing observations, I imputed them hierarchically: first via forward fill (using the previous month's value), then backward fill (using the next month's values), and finally using the overall mean or zero (if it made logical sense) for any remaining missing entries. To get a sense of the weather data, let's take a look at the different variables that were tracked. Since there are more than 10+ variables, and not all of them were consistently recorded, I'll present a table below showing the features that I plan to include in my modeling and analysis part.

<br>

<table style="border-collapse: collapse; width: 50%; margin-left: auto; margin-right: auto;">
  <thead style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <th style="border: 1px solid black; text-align: center; padding: 8px;">Variable</th>
      <th style="border: 1px solid black; text-align: center; padding: 8px;">Mean</th>
      <th style="border: 1px solid black; text-align: center; padding: 8px;">Median</th>
      <th style="border: 1px solid black; text-align: center; padding: 8px;">Standard Deviation</th>
      <th style="border: 1px solid black; text-align: center;">Minimum</th>
      <th style="border: 1px solid black; text-align: center;">Maximum</th>
      <th style="border: 1px solid black; text-align: center;">Units</th>
    </tr>
  </thead>
  <tbody style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthTemp</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">58.07</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">56.41</td>
      <td style="border: 1px solid black; text-align: center;">15.47</td>
      <td style="border: 1px solid black; text-align: center;">28.81</td>
      <td style="border: 1px solid black; text-align: center;">152.80</td>
      <td style="border: 1px solid black; text-align: center;">F</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthTempLow</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">47.45</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">46.05</td>
      <td style="border: 1px solid black; text-align: center;">12.06</td>
      <td style="border: 1px solid black; text-align: center;">18.53</td>
      <td style="border: 1px solid black; text-align: center;">140.73</td>
      <td style="border: 1px solid black; text-align: center;">F</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthTempHigh</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">68.69</td>
      <td style="border: 1px solid black; text-align: center;">66.88</td>
      <td style="border: 1px solid black; text-align: center;">20.25</td>
      <td style="border: 1px solid black; text-align: center;">34.95</td>
      <td style="border: 1px solid black; text-align: center;">173.50</td>
      <td style="border: 1px solid black; text-align: center;">F</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthDew</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">46.23</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">46.25</td>
      <td style="border: 1px solid black; text-align: center;">8.62</td>
      <td style="border: 1px solid black; text-align: center;">18.59</td>
      <td style="border: 1px solid black; text-align: center;">69.46</td>
      <td style="border: 1px solid black; text-align: center;">F</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthDewLow</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">41.46</td>
      <td style="border: 1px solid black; text-align: center;">41.29</td>
      <td style="border: 1px solid black; text-align: center;">8.81</td>
      <td style="border: 1px solid black; text-align: center;">10.23</td>
      <td style="border: 1px solid black; text-align: center;">64.75</td>
      <td style="border: 1px solid black; text-align: center;">F</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthDewHigh</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">51.00</td>
      <td style="border: 1px solid black; text-align: center;">51.16</td>
      <td style="border: 1px solid black; text-align: center;">8.62</td>
      <td style="border: 1px solid black; text-align: center;">19.40</td>
      <td style="border: 1px solid black; text-align: center;">79.79</td>
      <td style="border: 1px solid black; text-align: center;">F</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthWindSpd</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">7.30</td>
      <td style="border: 1px solid black; text-align: center;">7.09</td>
      <td style="border: 1px solid black; text-align: center;">1.79</td>
      <td style="border: 1px solid black; text-align: center;">0.86</td>
      <td style="border: 1px solid black; text-align: center;">23.73</td>
      <td style="border: 1px solid black; text-align: center;">mph</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthVis</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">7.32</td>
      <td style="border: 1px solid black; text-align: center;">6.31</td>
      <td style="border: 1px solid black; text-align: center;">3.30</td>
      <td style="border: 1px solid black; text-align: center;">1.98</td>
      <td style="border: 1px solid black; text-align: center;">25.13</td>
      <td style="border: 1px solid black; text-align: center;">mi</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthMinVis</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">4.56</td>
      <td style="border: 1px solid black; text-align: center;">4.40</td>
      <td style="border: 1px solid black; text-align: center;">1.83</td>
      <td style="border: 1px solid black; text-align: center;">0.39</td>
      <td style="border: 1px solid black; text-align: center;">15.56</td>
      <td style="border: 1px solid black; text-align: center;">mi</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthMaxVis</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">10.08</td>
      <td style="border: 1px solid black; text-align: center;">9.77</td>
      <td style="border: 1px solid black; text-align: center;">6.02</td>
      <td style="border: 1px solid black; text-align: center;">3.28</td>
      <td style="border: 1px solid black; text-align: center;">40.49</td>
      <td style="border: 1px solid black; text-align: center;">mi</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthPressure</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">30.03</td>
      <td style="border: 1px solid black; text-align: center;">30.01</td>
      <td style="border: 1px solid black; text-align: center;">0.17</td>
      <td style="border: 1px solid black; text-align: center;">28.95</td>
      <td style="border: 1px solid black; text-align: center;">34.78</td>
      <td style="border: 1px solid black; text-align: center;">Hg</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthMinPressure</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">29.95</td>
      <td style="border: 1px solid black; text-align: center;">29.94</td>
      <td style="border: 1px solid black; text-align: center;">0.14</td>
      <td style="border: 1px solid black; text-align: center;">27.30</td>
      <td style="border: 1px solid black; text-align: center;">30.42</td>
      <td style="border: 1px solid black; text-align: center;">Hg</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">AvgMonthMaxPressure</td>
      <td style="border: 1px solid black; text-align: center; padding: 8px;">30.11</td>
      <td style="border: 1px solid black; text-align: center;">30.08</td>
      <td style="border: 1px solid black; text-align: center;">0.29</td>
      <td style="border: 1px solid black; text-align: center;">29.72</td>
      <td style="border: 1px solid black; text-align: center;">39.75</td>
      <td style="border: 1px solid black; text-align: center;">Hg</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">MaxMonthTempHigh</td>
      <td style="border: 1px solid black; text-align: center;">84.38</td>
      <td style="border: 1px solid black; text-align: center;">80.60</td>
      <td style="border: 1px solid black; text-align: center;">25.95</td>
      <td style="border: 1px solid black; text-align: center;">42.80</td>
      <td style="border: 1px solid black; text-align: center;">206.60</td>
      <td style="border: 1px solid black; text-align: center;">F</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">MaxMonthDewHigh</td>
      <td style="border: 1px solid black; text-align: center;">60.71</td>
      <td style="border: 1px solid black; text-align: center;">60.08</td>
      <td style="border: 1px solid black; text-align: center;">11.57</td>
      <td style="border: 1px solid black; text-align: center;">19.40</td>
      <td style="border: 1px solid black; text-align: center;">210.20</td>
      <td style="border: 1px solid black; text-align: center;">F</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">MaxMonthMaxWindSpd</td>
      <td style="border: 1px solid black; text-align: center;">26.21</td>
      <td style="border: 1px solid black; text-align: center;">24.17</td>
      <td style="border: 1px solid black; text-align: center;">13.04</td>
      <td style="border: 1px solid black; text-align: center;">3.45</td>
      <td style="border: 1px solid black; text-align: center;">391.26</td>
      <td style="border: 1px solid black; text-align: center;">mph</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">MaxMonthMaxPressure</td>
      <td style="border: 1px solid black; text-align: center;">30.73</td>
      <td style="border: 1px solid black; text-align: center;">30.35</td>
      <td style="border: 1px solid black; text-align: center;">7.46</td>
      <td style="border: 1px solid black; text-align: center;">29.98</td>
      <td style="border: 1px solid black; text-align: center;">295.27</td>
      <td style="border: 1px solid black; text-align: center;">Hg</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">MaxMonthSnowDepth</td>
      <td style="border: 1px solid black; text-align: center;">0.66</td>
      <td style="border: 1px solid black; text-align: center;">0.00</td>
      <td style="border: 1px solid black; text-align: center;">4.03</td>
      <td style="border: 1px solid black; text-align: center;">0.00</td>
      <td style="border: 1px solid black; text-align: center;">92.52</td>
      <td style="border: 1px solid black; text-align: center;">in</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">MaxMonthPrecip</td>
      <td style="border: 1px solid black; text-align: center;">0.34</td>
      <td style="border: 1px solid black; text-align: center;">0.05</td>
      <td style="border: 1px solid black; text-align: center;">0.58</td>
      <td style="border: 1px solid black; text-align: center;">0.00</td>
      <td style="border: 1px solid black; text-align: center;">6.05</td>
      <td style="border: 1px solid black; text-align: center;">in</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">MinMonthTempLow</td>
      <td style="border: 1px solid black; text-align: center;">36.33</td>
      <td style="border: 1px solid black; text-align: center;">35.60</td>
      <td style="border: 1px solid black; text-align: center;">11.16</td>
      <td style="border: 1px solid black; text-align: center;">-5.80</td>
      <td style="border: 1px solid black; text-align: center;">68.00</td>
      <td style="border: 1px solid black; text-align: center;">F</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">MinMonthDewLow</td>
      <td style="border: 1px solid black; text-align: center;">26.73</td>
      <td style="border: 1px solid black; text-align: center;">28.04</td>
      <td style="border: 1px solid black; text-align: center;">14.53</td>
      <td style="border: 1px solid black; text-align: center;">-142.60</td>
      <td style="border: 1px solid black; text-align: center;">55.40</td>
      <td style="border: 1px solid black; text-align: center;">F</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">MinMonthMinPressure</td>
      <td style="border: 1px solid black; text-align: center;">29.46</td>
      <td style="border: 1px solid black; text-align: center;">29.62</td>
      <td style="border: 1px solid black; text-align: center;">1.84</td>
      <td style="border: 1px solid black; text-align: center;">0.00</td>
      <td style="border: 1px solid black; text-align: center;">30.21</td>
      <td style="border: 1px solid black; text-align: center;">Hg</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">SumMonthPrecip</td>
      <td style="border: 1px solid black; text-align: center;">0.88</td>
      <td style="border: 1px solid black; text-align: center;">0.00</td>
      <td style="border: 1px solid black; text-align: center;">2.08</td>
      <td style="border: 1px solid black; text-align: center;">0.00</td>
      <td style="border: 1px solid black; text-align: center;">17.99</td>
      <td style="border: 1px solid black; text-align: center;">in</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">SumMonthSnowDepth</td>
      <td style="border: 1px solid black; text-align: center;">0.18</td>
      <td style="border: 1px solid black; text-align: center;">0.00</td>
      <td style="border: 1px solid black; text-align: center;">2.81</td>
      <td style="border: 1px solid black; text-align: center;">0.00</td>
      <td style="border: 1px solid black; text-align: center;">92.52</td>
      <td style="border: 1px solid black; text-align: center;">in</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: left; padding: 8px;">DaysRainMonth</td>
      <td style="border: 1px solid black; text-align: center;">3.72</td>
      <td style="border: 1px solid black; text-align: center;">0.00</td>
      <td style="border: 1px solid black; text-align: center;">6.18</td>
      <td style="border: 1px solid black; text-align: center;">0.00</td>
      <td style="border: 1px solid black; text-align: center;">30.00</td>
      <td style="border: 1px solid black; text-align: center;">days</td>
    </tr>
  </tbody>
</table>

#### Basic Descriptives

Let's also explore the data a bit more by looking at how VintageScores changes based on wine type.

<br>

<table style="border-collapse: collapse; width: 50%; margin-left: auto; margin-right: auto;">
  <thead style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <th style="border: 1px solid black; text-align: center;">WineType</th>
      <th style="border: 1px solid black; text-align: center;">Mean VintageScore</th>
    </tr>
  </thead>
  <tbody style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Amarone</td>
      <td style="border: 1px solid black; text-align: center;">90.62</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Barolo</td>
      <td style="border: 1px solid black; text-align: center;">94.05</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Bolgheri</td>
      <td style="border: 1px solid black; text-align: center;">91.42</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Cabernet Sauvignon</td>
      <td style="border: 1px solid black; text-align: center;">91.86</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Chablis</td>
      <td style="border: 1px solid black; text-align: center;">93.04</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Chardonnay</td>
      <td style="border: 1px solid black; text-align: center;">91.18</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Chenin Blanc</td>
      <td style="border: 1px solid black; text-align: center;">92.00</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Chianti</td>
      <td style="border: 1px solid black; text-align: center;">91.62</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Gamay</td>
      <td style="border: 1px solid black; text-align: center;">91.38</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Gewurztraminer</td>
      <td style="border: 1px solid black; text-align: center;">91.27</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Merlot</td>
      <td style="border: 1px solid black; text-align: center;">93.00</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Pinot Noir</td>
      <td style="border: 1px solid black; text-align: center;">91.95</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Semillon</td>
      <td style="border: 1px solid black; text-align: center;">92.54</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Soave</td>
      <td style="border: 1px solid black; text-align: center;">90.15</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Syrah</td>
      <td style="border: 1px solid black; text-align: center;">92.79</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Zinfandel</td>
      <td style="border: 1px solid black; text-align: center;">90.04</td>
    </tr>
  </tbody>
</table>

<br>

It's also interesting to look at the highest VintageScores given to each wine.

<br>

<table style="border-collapse: collapse; width: 50%; margin-left: auto; margin-right: auto;">
  <thead style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <th style="border: 1px solid black; text-align: center;">WineType</th>
      <th style="border: 1px solid black; text-align: center;">Max VintageScore</th>
    </tr>
  </thead>
  <tbody style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Amarone</td>
      <td style="border: 1px solid black; text-align: center;">94</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Barolo</td>
      <td style="border: 1px solid black; text-align: center;">99</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Bolgheri</td>
      <td style="border: 1px solid black; text-align: center;">97</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Cabernet Sauvignon</td>
      <td style="border: 1px solid black; text-align: center;">100</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Chablis</td>
      <td style="border: 1px solid black; text-align: center;">96</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Chardonnay</td>
      <td style="border: 1px solid black; text-align: center;">96</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Chenin Blanc</td>
      <td style="border: 1px solid black; text-align: center;">96</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Chianti</td>
      <td style="border: 1px solid black; text-align: center;">96</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Gamay</td>
      <td style="border: 1px solid black; text-align: center;">96</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Gewurztraminer</td>
      <td style="border: 1px solid black; text-align: center;">95</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Merlot</td>
      <td style="border: 1px solid black; text-align: center;">98</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Pinot Noir</td>
      <td style="border: 1px solid black; text-align: center;">98</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Semillon</td>
      <td style="border: 1px solid black; text-align: center;">96</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Soave</td>
      <td style="border: 1px solid black; text-align: center;">94</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Syrah</td>
      <td style="border: 1px solid black; text-align: center;">99</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Zinfandel</td>
      <td style="border: 1px solid black; text-align: center;">94</td>
    </tr>
  </tbody>
</table>

<br>

Finally, how does VintageScore vary based on the year?

<br>

<table style="border-collapse: collapse; width: 50%; margin-left: auto; margin-right: auto;">
  <thead style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <th style="border: 1px solid black; text-align: center;">Year</th>
      <th style="border: 1px solid black; text-align: center;">Mean VintageScore</th>
    </tr>
  </thead>
  <tbody style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">1998</td>
      <td style="border: 1px solid black; text-align: center;">88.91</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">1999</td>
      <td style="border: 1px solid black; text-align: center;">89.41</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2000</td>
      <td style="border: 1px solid black; text-align: center;">88.23</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2001</td>
      <td style="border: 1px solid black; text-align: center;">91.86</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2002</td>
      <td style="border: 1px solid black; text-align: center;">88.62</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2003</td>
      <td style="border: 1px solid black; text-align: center;">88.90</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2004</td>
      <td style="border: 1px solid black; text-align: center;">91.04</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2005</td>
      <td style="border: 1px solid black; text-align: center;">92.17</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2006</td>
      <td style="border: 1px solid black; text-align: center;">90.13</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2007</td>
      <td style="border: 1px solid black; text-align: center;">91.61</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2008</td>
      <td style="border: 1px solid black; text-align: center;">90.74</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2009</td>
      <td style="border: 1px solid black; text-align: center;">92.94</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2010</td>
      <td style="border: 1px solid black; text-align: center;">93.09</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2011</td>
      <td style="border: 1px solid black; text-align: center;">91.09</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2012</td>
      <td style="border: 1px solid black; text-align: center;">92.23</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2013</td>
      <td style="border: 1px solid black; text-align: center;">91.64</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2014</td>
      <td style="border: 1px solid black; text-align: center;">91.91</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2015</td>
      <td style="border: 1px solid black; text-align: center;">93.95</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2016</td>
      <td style="border: 1px solid black; text-align: center;">93.91</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2017</td>
      <td style="border: 1px solid black; text-align: center;">92.04</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2018</td>
      <td style="border: 1px solid black; text-align: center;">92.78</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2019</td>
      <td style="border: 1px solid black; text-align: center;">93.91</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2020</td>
      <td style="border: 1px solid black; text-align: center;">92.09</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2021</td>
      <td style="border: 1px solid black; text-align: center;">93.26</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2022</td>
      <td style="border: 1px solid black; text-align: center;">92.87</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">2023</td>
      <td style="border: 1px solid black; text-align: center;">93.35</td>
    </tr>
  </tbody>
</table>

<br>

A useful plot that I always like to perform is a correlation heatmap of my predictor variables of interest with the main dependent variable, VintageScore.

<p align="center">
    <img src="../assets/img/projects/wine/correlation_heatmap.png" alt="HeatmapVintageScore" width="400">
</p>
<br>

The variables that had a significant correlation with VintageScore are denoted with the \*, \*\*, or \*\*\*, representing significance at the <0.05, <0.01, or <0.001 level respectively.

<br>

Interestingly, we see that AvgMonthWindSpd, or the average monthly wind speed, is positively related to VintageScore. This means that the higher the monthly wind speed, the higher the vintage score. While I know nothing about growing vines, this may seem quite counter-intuitive because the higher the wind speed, the more potential damage to the grape vines. However, some quick research suggests that more wind could act as a proxy for good air circulation, which benefits the grape vines by drying them (preventing damp conditions) and moderating temperatures.

<br>

We also see that DaysRainMonth, so the number of days where it rained in that month, is negatively associated with VintageScore. This means that the more that it rained in a month, the lower the VintageScore. This could make sense because too much rain could mean that 1) the grape vines have more susceptibility to diseases (moisture for fungal spores) and 2) less sunny days so less photosynthesis.

<br>

So next, we'll move onto building a time series model that can potentially predict vintage scores!
<br>
[Go to Part 2 Forecasting](/pages/wine-part2)
<br>
[Go to Part 3 Important Features](/pages/wine-part3)

