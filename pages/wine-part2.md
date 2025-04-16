---
layout: page
title: Forecasting Wine Vintage Scores Detail
permalink: /pages/wine-part2
---

## Forecasting and Analyzing Wine Vintage Scores Part 2

[Return to Part 1](/projects/wine)
<br>
[Go to Part 3 Important Features](/pages/wine-part3)

# Forecasting

Since my goal is to see if weather can be used to predict wine vintage scores, there are various modeling approaches available. In this project, I'm going to opt for time series models due to the nature of the data (i.e. the yearly and seasonly weather pattern/cycles). 

### Pinot Noir

To simplify the initial time series modeling, I'll focus on analyzing a single wine type for now: Pinot Noir (because it's my favorite grape). Additionally, I'll filter for Pinot Noir vintage scores in the Northern Hemisphere. This is to avoid the complexity related to differing agricultural cycles (Southern Hemisphere seasons are flipped so the growing/harvesting periods are different). A crucial preliminary step in times series modeling is to assess the stationarity of the dependent variable, or whether the basic statistical properties do not change over time. This means that VintageScore should have constant mean and variance. We can visually inspect this by looking at whether there is a clear upward/downward trend line and whether there is seasonality cycles.  

<br>
<p align="center">
    <img src="../assets/img/projects/wine/PN_decompose.png" alt="PN_decompose" width="400">
</p>
<br>

Based on the decomposition, we can see that there could be a long term trend, but it's hard to say. We definitely see seasonality patterns though. The next step after visual decomposition is to run a statistical test to check for stationarity, which is the ADF test.

<br>

ADF Test for WineType: Pinot Noir (Original Series)
ADF Statistic: -2.8083829632432664
p-value: 0.057086262941437045
Critical Values: {'1%': -3.437423894618058, '5%': -2.864662884591462, '10%': -2.5684328157550835}
VintageScore is likely non-stationary for this group (Original Series).

<br>

The test suggests that VintageScore is likely non-stationary, and so would require some differencing (i.e. subtracting VintageScore with its lagged self) when applying time series modeling. 

<br> 

The next step is to identify the underlying temporal structure of VintageScore. In other words, we want to understand how the score in a given year relates to scores in previous years. The standard way to do this is to look at the Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) plots. 
- The ACF plot visualizes the correlation between the time series and lagged versions of itself at different time lags (k). It represents the total correlation between the observation at time t and the observation at time t-k. *Looking at the ACF plot helps us pick the q parameter in time series modeling. This q parameter is called the MA or moving average and represents the number of past forecast errors or random shocks that need to be accounted for.*
- The PACF plot visualizes the partial or direct correlation between the observation at time t and the observation at time t-k after controlling for the effects of the correlations in between. *Looking at the PACF plot helps us pick the p parameter, which is called AR or autoregressive and represents the number of past observations that are considered in the time series modeling.*

<br> 
<p align="center">
    <img src="../assets/img/projects/wine/PN_ACF_Differenced.png" alt="PN_ACF" width="400">
</p>
<br>

Based on the ACF plot, it seems like there is a significant spike at lag 1, so it may suggest that the Moving Average parameter, q, is 1. 

<br>
<p align="center">
    <img src="../assets/img/projects/wine/PN_PACF_Differenced.png" alt="PN_PACF" width="400">
</p>
<br>

Based on the PACF plot, it seems like there is a significant spike at lag 5 and 7, so it may suggest that the Autoregressive parameter, p, is 5 or 7. 

<br>

#### ARIMAX

Finally, we can get to the modeling! I'll model VintageScore first using an **ARIMAX** (Autoregressive Integrated Moving Average with eXogenous variables) model. This model structure allows VintageScore to be predicted based on its own past values, past errors and additional weather features. For an ARIMAX model, we need to choose 3 parameters, p, d, and q. Based on the analysis above, We might choose p = 5, d = 1, and q = 1. However, I ran this model and found that the model didn't converge. Instead, I discovered that an ARIMAX (p = 1, d = 1, and q = 1) model converged.
                               
==============================================================================
Dep. Variable:           VintageScore   No. Observations:                  828
Model:                 ARIMA(1, 1, 1)   Log Likelihood               -1942.876
Date:                Wed, 16 Apr 2025   AIC                           3913.752
Time:                        12:30:17   BIC                           3979.801
Sample:                             0   HQIC                          3939.086
                                - 828                                         
Covariance Type:                  opg                                         
=======================================================================================
                          coef    std err          z      P>|z|      [0.025      0.975]
---------------------------------------------------------------------------------------
AvgMonthWindSpd         0.2798      0.158      1.776      0.076      -0.029       0.589
MaxMonthDewHigh         0.2642      0.209      1.265      0.206      -0.145       0.674
AvgMonthMaxPressure     0.2483      0.318      0.782      0.434      -0.374       0.871
MaxMonthPrecip          0.0266      0.113      0.236      0.813      -0.194       0.247
MinMonthMinPressure    -0.7424      0.983     -0.755      0.450      -2.670       1.185
SumMonthSnowDepth       2.1032      1.065      1.975      0.048       0.016       4.190
MaxMonthTempHigh        0.0255      0.179      0.143      0.886      -0.325       0.376
AvgMonthVis            -0.2104      0.162     -1.300      0.194      -0.527       0.107
DaysRainMonth          -0.0028      0.117     -0.024      0.981      -0.233       0.227
RegionTag_KSTS          0.5794      0.266      2.176      0.030       0.058       1.101
RegionTag_LFSD          1.9893      0.315      6.319      0.000       1.372       2.606
ar.L1                  -0.3001      0.035     -8.455      0.000      -0.370      -0.231
ma.L1                  -0.8730      0.017    -51.616      0.000      -0.906      -0.840
sigma2                  6.4129      0.346     18.539      0.000       5.735       7.091
===================================================================================
Ljung-Box (L1) (Q):                   2.32   Jarque-Bera (JB):                 1.19
Prob(Q):                              0.13   Prob(JB):                         0.55
Heteroskedasticity (H):               0.50   Skew:                            -0.09
Prob(H) (two-sided):                  0.00   Kurtosis:                         2.97
===================================================================================

#### SARIMAX 

I'll also try a Seasonal ARIMAX model which uses the ARIMAX farmework but adds parameters designed to capture seasonal patterns.

<br>

Model results
Actual vs Predicted graph

### Cabernet Sauvignon

For good measure, I'll repeat the same procedure with a different wine type, Cabernet Sauvignon, my second favorite grape.

Tried with all wine types but model could not converge

<br>
[Return to Part 1](/projects/wine)
<br>
[Go to Part 3 Important Features](/pages/wine-part3)



