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

<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Mean</th>
      <th>Median</th>
      <th>Standard Deviation</th>
      <th>Minimum</th>
      <th>Maximum</th>
      <th>Units</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>AvgMonthTemp</td>
      <td>58.07</td>
      <td>56.41</td>
      <td>15.47</td>
      <td>28.81</td>
      <td>152.80</td>
      <td>F</td>
    </tr>
    <tr>
      <td>AvgMonthTempLow</td>
      <td>47.45</td>
      <td>46.05</td>
      <td>12.06</td>
      <td>18.53</td>
      <td>140.73</td>
      <td>F</td>
    </tr>
    <tr>
      <td>AvgMonthTempHigh</td>
      <td>68.69</td>
      <td>66.88</td>
      <td>20.25</td>
      <td>34.95</td>
      <td>173.50</td>
      <td>F</td>
    </tr>
    <tr>
      <td>AvgMonthDew</td>
      <td>46.23</td>
      <td>46.25</td>
      <td>8.62</td>
      <td>18.59</td>
      <td>69.46</td>
      <td>F</td>
    </tr>
    <tr>
      <td>AvgMonthDewLow</td>
      <td>41.46</td>
      <td>41.29</td>
      <td>8.81</td>
      <td>10.23</td>
      <td>64.75</td>
      <td>F</td>
    </tr>
    <tr>
      <td>AvgMonthDewHigh</td>
      <td>51.00</td>
      <td>51.16</td>
      <td>8.62</td>
      <td>19.40</td>
      <td>79.79</td>
      <td>F</td>
    </tr>
    <tr>
      <td>AvgMonthWindSpd</td>
      <td>7.30</td>
      <td>7.09</td>
      <td>1.79</td>
      <td>0.86</td>
      <td>23.73</td>
      <td>mph</td>
    </tr>
    <tr>
      <td>AvgMonthVis</td>
      <td>7.32</td>
      <td>6.31</td>
      <td>3.30</td>
      <td>1.98</td>
      <td>25.13</td>
      <td>mi</td>
    </tr>
    <tr>
      <td>AvgMonthMinVis</td>
      <td>4.56</td>
      <td>4.40</td>
      <td>1.83</td>
      <td>0.39</td>
      <td>15.56</td>
      <td>mi</td>
    </tr>
    <tr>
      <td>AvgMonthMaxVis</td>
      <td>10.08</td>
      <td>9.77</td>
      <td>6.02</td>
      <td>3.28</td>
      <td>40.49</td>
      <td>mi</td>
    </tr>
    <tr>
      <td>AvgMonthPressure</td>
      <td>30.03</td>
      <td>30.01</td>
      <td>0.17</td>
      <td>28.95</td>
      <td>34.78</td>
      <td>Hg</td>
    </tr>
    <tr>
      <td>AvgMonthMinPressure</td>
      <td>29.95</td>
      <td>29.94</td>
      <td>0.14</td>
      <td>27.30</td>
      <td>30.42</td>
      <td>Hg</td>
    </tr>
    <tr>
      <td>AvgMonthMaxPressure</td>
      <td>30.11</td>
      <td>30.08</td>
      <td>0.29</td>
      <td>29.72</td>
      <td>39.75</td>
      <td>Hg</td>
    </tr>
    <tr>
      <td>MaxMonthTempHigh</td>
      <td>84.38</td>
      <td>80.60</td>
      <td>25.95</td>
      <td>42.80</td>
      <td>206.60</td>
      <td>F</td>
    </tr>
    <tr>
      <td>MaxMonthDewHigh</td>
      <td>60.71</td>
      <td>60.08</td>
      <td>11.57</td>
      <td>19.40</td>
      <td>210.20</td>
      <td>F</td>
    </tr>
    <tr>
      <td>MaxMonthMaxWindSpd</td>
      <td>26.21</td>
      <td>24.17</td>
      <td>13.04</td>
      <td>3.45</td>
      <td>391.26</td>
      <td>mph</td>
    </tr>
    <tr>
      <td>MaxMonthMaxPressure</td>
      <td>30.73</td>
      <td>30.35</td>
      <td>7.46</td>
      <td>29.98</td>
      <td>295.27</td>
      <td>Hg</td>
    </tr>
    <tr>
      <td>MaxMonthSnowDepth</td>
      <td>0.66</td>
      <td>0.00</td>
      <td>4.03</td>
      <td>0.00</td>
      <td>92.52</td>
      <td>in</td>
    </tr>
    <tr>
      <td>MaxMonthPrecip</td>
      <td>0.34</td>
      <td>0.05</td>
      <td>0.58</td>
      <td>0.00</td>
      <td>6.05</td>
      <td>in</td>
    </tr>
    <tr>
      <td>MinMonthTempLow</td>
      <td>36.33</td>
      <td>35.60</td>
      <td>11.16</td>
      <td>-5.80</td>
      <td>68.00</td>
      <td>F</td>
    </tr>
    <tr>
      <td>MinMonthDewLow</td>
      <td>26.73</td>
      <td>28.04</td>
      <td>14.53</td>
      <td>-142.60</td>
      <td>55.40</td>
      <td>F</td>
    </tr>
    <tr>
      <td>MinMonthMinPressure</td>
      <td>29.46</td>
      <td>29.62</td>
      <td>1.84</td>
      <td>0.00</td>
      <td>30.21</td>
      <td>Hg</td>
    </tr>
    <tr>
      <td>SumMonthPrecip</td>
      <td>0.88</td>
      <td>0.00</td>
      <td>2.08</td>
      <td>0.00</td>
      <td>17.99</td>
      <td>in</td>
    </tr>
    <tr>
      <td>SumMonthSnowDepth</td>
      <td>0.18</td>
      <td>0.00</td>
      <td>2.81</td>
      <td>0.00</td>
      <td>92.52</td>
      <td>in</td>
    </tr>
    <tr>
      <td>DaysRainMonth</td>
      <td>3.72</td>
      <td>0.00</td>
      <td>6.18</td>
      <td>0.00</td>
      <td>30.00</td>
      <td>days</td>
    </tr>
  </tbody>
</table>

#### Basic Descriptives

Let's also explore the data a bit more by looking at how VintageScores changes based on wine type.

<br>

<table>
  <thead>
    <tr>
      <th>WineType</th>
      <th>Mean VintageScore</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Amarone</td>
      <td>90.62</td>
    </tr>
    <tr>
      <td>Barolo</td>
      <td>94.05</td>
    </tr>
    <tr>
      <td>Bolgheri</td>
      <td>91.42</td>
    </tr>
    <tr>
      <td>Cabernet Sauvignon</td>
      <td>91.86</td>
    </tr>
    <tr>
      <td>Chablis</td>
      <td>93.04</td>
    </tr>
    <tr>
      <td>Chardonnay</td>
      <td>91.18</td>
    </tr>
    <tr>
      <td>Chenin Blanc</td>
      <td>92.00</td>
    </tr>
    <tr>
      <td>Chianti</td>
      <td>91.62</td>
    </tr>
    <tr>
      <td>Gamay</td>
      <td>91.38</td>
    </tr>
    <tr>
      <td>Gewurztraminer</td>
      <td>91.27</td>
    </tr>
    <tr>
      <td>Merlot</td>
      <td>93.00</td>
    </tr>
    <tr>
      <td>Pinot Noir</td>
      <td>91.95</td>
    </tr>
    <tr>
      <td>Semillon</td>
      <td>92.54</td>
    </tr>
    <tr>
      <td>Soave</td>
      <td>90.15</td>
    </tr>
    <tr>
      <td>Syrah</td>
      <td>92.79</td>
    </tr>
    <tr>
      <td>Zinfandel</td>
      <td>90.04</td>
    </tr>
  </tbody>
</table>

<br>

It's also interesting to look at the highest VintageScores given to each wine.

<br>

<table>
  <thead>
    <tr>
      <th>WineType</th>
      <th>Max VintageScore</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Amarone</td>
      <td>94</td>
    </tr>
    <tr>
      <td>Barolo</td>
      <td>99</td>
    </tr>
    <tr>
      <td>Bolgheri</td>
      <td>97</td>
    </tr>
    <tr>
      <td>Cabernet Sauvignon</td>
      <td>100</td>
    </tr>
    <tr>
      <td>Chablis</td>
      <td>96</td>
    </tr>
    <tr>
      <td>Chardonnay</td>
      <td>96</td>
    </tr>
    <tr>
      <td>Chenin Blanc</td>
      <td>96</td>
    </tr>
    <tr>
      <td>Chianti</td>
      <td>96</td>
    </tr>
    <tr>
      <td>Gamay</td>
      <td>96</td>
    </tr>
    <tr>
      <td>Gewurztraminer</td>
      <td>95</td>
    </tr>
    <tr>
      <td>Merlot</td>
      <td>98</td>
    </tr>
    <tr>
      <td>Pinot Noir</td>
      <td>98</td>
    </tr>
    <tr>
      <td>Semillon</td>
      <td>96</td>
    </tr>
    <tr>
      <td>Soave</td>
      <td>94</td>
    </tr>
    <tr>
      <td>Syrah</td>
      <td>99</td>
    </tr>
    <tr>
      <td>Zinfandel</td>
      <td>94</td>
    </tr>
  </tbody>
</table>

<br>

Finally, how does VintageScore vary based on the year?

<br>

<table>
  <thead>
    <tr>
      <th>Year</th>
      <th>Mean VintageScore</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1998</td>
      <td>88.91</td>
    </tr>
    <tr>
      <td>1999</td>
      <td>89.41</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>88.23</td>
    </tr>
    <tr>
      <td>2001</td>
      <td>91.86</td>
    </tr>
    <tr>
      <td>2002</td>
      <td>88.62</td>
    </tr>
    <tr>
      <td>2003</td>
      <td>88.90</td>
    </tr>
    <tr>
      <td>2004</td>
      <td>91.04</td>
    </tr>
    <tr>
      <td>2005</td>
      <td>92.17</td>
    </tr>
    <tr>
      <td>2006</td>
      <td>90.13</td>
    </tr>
    <tr>
      <td>2007</td>
      <td>91.61</td>
    </tr>
    <tr>
      <td>2008</td>
      <td>90.74</td>
    </tr>
    <tr>
      <td>2009</td>
      <td>92.94</td>
    </tr>
    <tr>
      <td>2010</td>
      <td>93.09</td>
    </tr>
    <tr>
      <td>2011</td>
      <td>91.09</td>
    </tr>
    <tr>
      <td>2012</td>
      <td>92.23</td>
    </tr>
    <tr>
      <td>2013</td>
      <td>91.64</td>
    </tr>
    <tr>
      <td>2014</td>
      <td>91.91</td>
    </tr>
    <tr>
      <td>2015</td>
      <td>93.95</td>
    </tr>
    <tr>
      <td>2016</td>
      <td>93.91</td>
    </tr>
    <tr>
      <td>2017</td>
      <td>92.04</td>
    </tr>
    <tr>
      <td>2018</td>
      <td>92.78</td>
    </tr>
    <tr>
      <td>2019</td>
      <td>93.91</td>
    </tr>
    <tr>
      <td>2020</td>
      <td>92.09</td>
    </tr>
    <tr>
      <td>2021</td>
      <td>93.26</td>
    </tr>
    <tr>
      <td>2022</td>
      <td>92.87</td>
    </tr>
    <tr>
      <td>2023</td>
      <td>93.35</td>
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

