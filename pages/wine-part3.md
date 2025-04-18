---
layout: page
title: Analyzing Wine Vintage Factors
permalink: /pages/wine-part3
---

## Forecasting and Analyzing Wine Vintage Scores Part 3

[Return to Part 1](/projects/wine)
<br>
[Return to Part 2 Forecasting](/pages/wine-part2)

# Important Features

The previous time series analysis suggested specific weather variables—AvgMonthWindSpd and SumMonthSnowDepth for Pinot Noir, and AvgMonthVis and MaxMonthTempHigh for Cabernet Sauvignon—may play a role in influencing their respective vintage scores. This raises key questions: Are these effects specific to these grape varieties, or perhaps influenced by the specific ARIMAX/SARIMAX modeling framework used? 

<br>

To investigate weather impacts more broadly across all wine types and regions in the Northern Hemisphere data, I'll first run a multiple linear regression model incorporating all available weather features. I can then interpret the coefficients of the linear regression to assess which factors are significant in explaining vintage scores. Then, I'll employ a Random Forest model, which better captures non-linear patterns and use its feature importance rankings to identify weather variables that emerge as significant predictors of vintage scores.

### What weather factors affect wine vintage scores?

For both the linear regression and the Random Forest models developed in this section, the predictor set includes all available weather features from the current vintage year, as well as those same features lagged by one year. The purpose of incorporating these one-year lagged variables is to investigate potential carry-over effects from the previous year's conditions. This allows the models to assess whether factors like the vine's health status entering the season, which would be influenced by the prior year's weather, have a discernible impact on the current year's grape quality and resulting vintage score.

#### Linear Regression

For this linear regression model, I'm going to include fixed effect terms for the years and months. The reason for doing so is twofold. First, I want to control for the fact certain years might be better or worse than others in terms of wine quality. Second, I believe there is no strong reason to assume that each unit increase in year will have a uniform linear effect on the vintage score.

<br>
<table style="border-collapse: collapse; width: 90%; margin-left: auto; margin-right: auto;">
  <caption style="text-align: center; font-weight: bold;">OLS Regression Diagnostics</caption>
  <tbody style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">R-squared:</td>
      <td style="border: 1px solid black; text-align: center;">0.417</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Model:</td>
      <td style="border: 1px solid black; text-align: center;">OLS</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Adj. R-squared:</td>
      <td style="border: 1px solid black; text-align: center;">0.408</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Method:</td>
      <td style="border: 1px solid black; text-align: center;">Least Squares</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">F-statistic:</td>
      <td style="border: 1px solid black; text-align: center;">47.48</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Date:</td>
      <td style="border: 1px solid black; text-align: center;">Thu, 17 Apr 2025</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Prob (F-statistic):</td>
      <td style="border: 1px solid black; text-align: center;">0.00</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Time:</td>
      <td style="border: 1px solid black; text-align: center;">17:11:22</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Log-Likelihood:</td>
      <td style="border: 1px solid black; text-align: center;">-16347.</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">No. Observations:</td>
      <td style="border: 1px solid black; text-align: center;">7007</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AIC:</td>
      <td style="border: 1px solid black; text-align: center;">3.290e+04</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Df Residuals:</td>
      <td style="border: 1px solid black; text-align: center;">6902</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">BIC:</td>
      <td style="border: 1px solid black; text-align: center;">3.365e+04</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Df Model:</td>
      <td style="border: 1px solid black; text-align: center;">104</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">Covariance Type:</td>
      <td style="border: 1px solid black; text-align: center;">nonrobust</td>
    </tr>
  </tbody>
</table>
<br>

Looking at the model diagnostics, we see that the R^2 is 0.417. This means that the model is able to explain around 41% of the variance in VintageScore. Since the adjusted R^2 is 0.408 and is quite close to the R^2, it suggests that the model isn't overfitting too much by including a large number of irrelevant predictors.

<br>
<table style="border-collapse: collapse; width: 90%; margin-left: auto; margin-right: auto;">
  <caption style="text-align: center; font-weight: bold;">ARIMA(1, 1, 1) Results</caption>
  <thead style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <th style="border: 1px solid black; text-align: center;"></th>
      <th style="border: 1px solid black; text-align: center;">coef</th>
      <th style="border: 1px solid black; text-align: center;">std err</th>
      <th style="border: 1px solid black; text-align: center;">z</th>
      <th style="border: 1px solid black; text-align: center;">P&gt;|z|</th>
      <th style="border: 1px solid black; text-align: center;">[0.025</th>
      <th style="border: 1px solid black; text-align: center;">0.975]</th>
    </tr>
  </thead>
  <tbody style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthWindSpd</td>
      <td style="border: 1px solid black; text-align: center;">0.2798</td>
      <td style="border: 1px solid black; text-align: center;">0.158</td>
      <td style="border: 1px solid black; text-align: center;">1.776</td>
      <td style="border: 1px solid black; text-align: center;">0.076</td>
      <td style="border: 1px solid black; text-align: center;">-0.029</td>
      <td style="border: 1px solid black; text-align: center;">0.589</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">MaxMonthDewHigh</td>
      <td style="border: 1px solid black; text-align: center;">0.2642</td>
      <td style="border: 1px solid black; text-align: center;">0.209</td>
      <td style="border: 1px solid black; text-align: center;">1.265</td>
      <td style="border: 1px solid black; text-align: center;">0.206</td>
      <td style="border: 1px solid black; text-align: center;">-0.145</td>
      <td style="border: 1px solid black; text-align: center;">0.674</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthMaxPressure</td>
      <td style="border: 1px solid black; text-align: center;">0.2483</td>
      <td style="border: 1px solid black; text-align: center;">0.318</td>
      <td style="border: 1px solid black; text-align: center;">0.782</td>
      <td style="border: 1px solid black; text-align: center;">0.434</td>
      <td style="border: 1px solid black; text-align: center;">-0.374</td>
      <td style="border: 1px solid black; text-align: center;">0.871</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">MaxMonthPrecip</td>
      <td style="border: 1px solid black; text-align: center;">0.0266</td>
      <td style="border: 1px solid black; text-align: center;">0.113</td>
      <td style="border: 1px solid black; text-align: center;">0.236</td>
      <td style="border: 1px solid black; text-align: center;">0.813</td>
      <td style="border: 1px solid black; text-align: center;">-0.194</td>
      <td style="border: 1px solid black; text-align: center;">0.247</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">MinMonthMinPressure</td>
      <td style="border: 1px solid black; text-align: center;">-0.7424</td>
      <td style="border: 1px solid black; text-align: center;">0.983</td>
      <td style="border: 1px solid black; text-align: center;">-0.755</td>
      <td style="border: 1px solid black; text-align: center;">0.450</td>
      <td style="border: 1px solid black; text-align: center;">-2.670</td>
      <td style="border: 1px solid black; text-align: center;">1.185</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">SumMonthSnowDepth</td>
      <td style="border: 1px solid black; text-align: center;">2.1032</td>
      <td style="border: 1px solid black; text-align: center;">1.065</td>
      <td style="border: 1px solid black; text-align: center;">1.975</td>
      <td style="border: 1px solid black; text-align: center;">0.048</td>
      <td style="border: 1px solid black; text-align: center;">0.016</td>
      <td style="border: 1px solid black; text-align: center;">4.190</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">MaxMonthTempHigh</td>
      <td style="border: 1px solid black; text-align: center;">0.0255</td>
      <td style="border: 1px solid black; text-align: center;">0.179</td>
      <td style="border: 1px solid black; text-align: center;">0.143</td>
      <td style="border: 1px solid black; text-align: center;">0.886</td>
      <td style="border: 1px solid black; text-align: center;">-0.325</td>
      <td style="border: 1px solid black; text-align: center;">0.376</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthVis</td>
      <td style="border: 1px solid black; text-align: center;">-0.2104</td>
      <td style="border: 1px solid black; text-align: center;">0.162</td>
      <td style="border: 1px solid black; text-align: center;">-1.300</td>
      <td style="border: 1px solid black; text-align: center;">0.194</td>
      <td style="border: 1px solid black; text-align: center;">-0.527</td>
      <td style="border: 1px solid black; text-align: center;">0.107</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">DaysRainMonth</td>
      <td style="border: 1px solid black; text-align: center;">-0.0028</td>
      <td style="border: 1px solid black; text-align: center;">0.117</td>
      <td style="border: 1px solid black; text-align: center;">-0.024</td>
      <td style="border: 1px solid black; text-align: center;">0.981</td>
      <td style="border: 1px solid black; text-align: center;">-0.233</td>
      <td style="border: 1px solid black; text-align: center;">0.227</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">RegionTag_KSTS</td>
      <td style="border: 1px solid black; text-align: center;">-1.9702</td>
      <td style="border: 1px solid black; text-align: center;">0.366</td>
      <td style="border: 1px solid black; text-align: center;">-5.386</td>
      <td style="border: 1px solid black; text-align: center;">0.000</td>
      <td style="border: 1px solid black; text-align: center;">-2.687</td>
      <td style="border: 1px solid black; text-align: center;">-1.253</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">RegionTag_LFBD</td>
      <td style="border: 1px solid black; text-align: center;">1.7317</td>
      <td style="border: 1px solid black; text-align: center;">0.331</td>
      <td style="border: 1px solid black; text-align: center;">5.228</td>
      <td style="border: 1px solid black; text-align: center;">0.000</td>
      <td style="border: 1px solid black; text-align: center;">1.082</td>
      <td style="border: 1px solid black; text-align: center;">2.381</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">ar.L1</td>
      <td style="border: 1px solid black; text-align: center;">-0.2524</td>
      <td style="border: 1px solid black; text-align: center;">0.033</td>
      <td style="border: 1px solid black; text-align: center;">-7.536</td>
      <td style="border: 1px solid black; text-align: center;">0.000</td>
      <td style="border: 1px solid black; text-align: center;">-0.318</td>
      <td style="border: 1px solid black; text-align: center;">-0.187</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">ma.L1</td>
      <td style="border: 1px solid black; text-align: center;">-0.8040</td>
      <td style="border: 1px solid black; text-align: center;">0.020</td>
      <td style="border: 1px solid black; text-align: center;">-39.620</td>
      <td style="border: 1px solid black; text-align: center;">0.000</td>
      <td style="border: 1px solid black; text-align: center;">-0.844</td>
      <td style="border: 1px solid black; text-align: center;">-0.764</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">sigma2</td>
      <td style="border: 1px solid black; text-align: center;">8.1224</td>
      <td style="border: 1px solid black; text-align: center;">0.371</td>
      <td style="border: 1px solid black; text-align: center;">21.874</td>
      <td style="border: 1px solid black; text-align: center;">0.000</td>
      <td style="border: 1px solid black; text-align: center;">7.395</td>
      <td style="border: 1px solid black; text-align: center;">8.850</td>
    </tr>
  </tbody>
</table>
<br>

Several weather variables showed statistically significant effects after controlling for  baseline differences associated with region, grape varietal, and year (via year dummies, which were mostly significant).
- Wind: A southerly wind direction (*WindDirectionT.S*, p=0.014) was positively associated with vintage scores. This aligns with the expectation that in the Northern Hemisphere, southerly winds often bring warmer air beneficial for ripening. However, *AvgMonthWindSpd* showed a significant negative coefficient (p=0.029). This contrasts with the positive association found in the Pinot Noir ARIMAX model. It's possible that while moderate wind benefits specific varieties like Pinot Noir (perhaps via disease reduction), higher average wind speeds across all varieties and regions might lead to detrimental effects
- Pressure: *MaxMonthMaxPressure* (maximum of daily highest pressure in a month) was marginally significant and positive (p=0.057), consistent with the idea that high-pressure systems could indicate stable, sunny weather conducive to good vintages. Its lagged version, MaxMonthMaxPressure_Lag (p=0.001), was also positive and significant, suggesting favorable weather conditions in the previous year may be beneficial to vine health and potential in the current year.
- Precipitation: *MaxMonthPrecip* (maximum inches of rain in a month) was positive and significant (p=0.034). This  might suggest that having at least one substantial rainfall event (perhaps breaking a dry spell or heatwave) during a key month is beneficial. The lagged effect of rain days (*DaysRainMonth_Lag*, p=0.012) was positive, suggesting that more frequent rainfall in the prior year is beneficial, likely due to recharging deep soil moisture reserves critical for the current season.
- Snow: *SumMonthSnowDepth* was significantly negative (p=0.014), indicating that higher snow accumulation is generally associated with lower scores in this broad analysis. This contrasts with the positive coefficient seen for Pinot Noir specifically, suggesting the general impact of heavy snow (e.g., potential for damage, delayed season start) might outweigh variety-specific benefits like insulation when averaged across all types.
- Temperature: The lagged minimum lowest daily temperature in a month (*MinMonthTempLow_Lag*, p=0.006) was positive and significant, possibly indicating that milder conditions (less extreme cold) in the preceding winter promote better vine health and subsequent vintage quality. 
- Dew Point: Both *MinMonthDewLow* (p=0.048), the minimum of the lowest daily dew point and its lag (*MinMonthDewLow_Lag*, p=0.013) were positive and significant. This might suggest that avoiding periods of extremely low dew points (which can indicate either very dry air causing stress, or correlate with very low nighttime temperatures increasing frost risk) is generally beneficial for vine health.

#### Random Forest

To complement the linear regression I  also trained a Random Forest model. For consistency, I used the same training/test splits employed for the time series models. I fine-tuned the hyperpamaters of the model using a Grid Search approach with cross-validation so that the selected parameters generalize well. The following key hyperparameters were optimized:
- n_estimators (Number of trees in the forest): 100, 200, 500
- max_depth (Maximum depth of individual trees): 5, 10, 20
- min_samples_split (Minimum number of samples required to split an internal node): 2, 5, 10
- min_samples_leaf (Minimum number of samples required to be at a leaf node): 1, 3, 5
- max_features (Number of features to consider when looking for the best split): sqrt of the total features, all features
- bootstrap (Method for sampling data points for training each tree): True, False

<br>

Following hyperparameter tuning and training, the best Random Forest model identified had a RMSE of 3.18. On the test set, this model achieved an RMSE of 1.86, which suggests good predictive accuracy.

<br>

A key advantage of Random Forest models is their ability to provide estimates of feature importance, which indicate how much each predictor variable contributes to the model's predictive accuracy. Essentially, feature importance is typically calculated by measuring how much the model's performance (or the purity of the tree nodes) decreases on average when the values of a specific feature are randomly shuffled.

<br>

Looking at the feature importance scores from the trained Random Forest model (excluding the fixed effect terms like year, month, wine type, and region tags, as these are controlled for), we can identify which weather variables the model found most influential:

<br>
<table style="border-collapse: collapse; width: 70%; margin-left: auto; margin-right: auto;">
  <caption style="text-align: center; font-weight: bold;">Top 20 Feature Importances (Excluding Fixed Effects)</caption>
  <thead style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <th style="border: 1px solid black; text-align: center;">Variable</th>
      <th style="border: 1px solid black; text-align: center;">Importance</th>
    </tr>
  </thead>
  <tbody style="border: 1px solid black;">
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthMaxVis_Lag</td>
      <td style="border: 1px solid black; text-align: center;">0.052291</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthVis_Lag</td>
      <td style="border: 1px solid black; text-align: center;">0.043316</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthMaxVis</td>
      <td style="border: 1px solid black; text-align: center;">0.032641</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthVis</td>
      <td style="border: 1px solid black; text-align: center;">0.024829</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">MaxMonthPrecip_Lag</td>
      <td style="border: 1px solid black; text-align: center;">0.021852</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">MaxMonthPrecip</td>
      <td style="border: 1px solid black; text-align: center;">0.020006</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthMinVis_Lag</td>
      <td style="border: 1px solid black; text-align: center;">0.016726</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthWindSpd_Lag</td>
      <td style="border: 1px solid black; text-align: center;">0.016238</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthWindSpd</td>
      <td style="border: 1px solid black; text-align: center;">0.016089</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthMinVis</td>
      <td style="border: 1px solid black; text-align: center;">0.015948</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthMaxPressure</td>
      <td style="border: 1px solid black; text-align: center;">0.012614</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">MaxMonthMaxPressure_Lag</td>
      <td style="border: 1px solid black; text-align: center;">0.012283</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthMinPressure</td>
      <td style="border: 1px solid black; text-align: center;">0.012211</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">MaxMonthTempHigh</td>
      <td style="border: 1px solid black; text-align: center;">0.012084</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthPressure</td>
      <td style="border: 1px solid black; text-align: center;">0.012026</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthMinPressure_Lag</td>
      <td style="border: 1px solid black; text-align: center;">0.011674</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthTemp</td>
      <td style="border: 1px solid black; text-align: center;">0.011430</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthPressure_Lag</td>
      <td style="border: 1px solid black; text-align: center;">0.011351</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">MaxMonthTempHigh_Lag</td>
      <td style="border: 1px solid black; text-align: center;">0.011220</td>
    </tr>
    <tr style="border: 1px solid black;">
      <td style="border: 1px solid black; text-align: center;">AvgMonthTempHigh</td>
      <td style="border: 1px solid black; text-align: center;">0.011173</td>
    </tr>
  </tbody>
</table>
<br>

Several key observations stand out from this list:
- Visibility: Variables related to visibility (average, max, min, both current and lagged) occupy the top four spots and appear multiple times in the top 10. This strongly suggests that atmospheric visibility is the most important weather-related factor for predicting vintage scores according to the Random Forest model.
- Precipitation Importance: The next most important category appears to be precipitation, specifically the maximum monthly precipitation (MaxMonthPrecip and its lag).
- Wind Speed: Wind speed variables (AvgMonthWindSpd and its lag) also feature relatively high on the list.

# Takeaways

The Random Forest model provided us with a ranking of weather features that contrasts with that of the linear regression model. In particular, visibility variables were not found to be statistically significant predictors. This difference could be a result of the ways these models operate: 
- Random Forests can capture complex non-linear relationships and interactions between variables automatically. Visibility's impact might be non-linear (e.g., important up to a certain threshold) or highly dependent on interactions with other factors (like temperature or humidity), which the linear model wouldn't easily detect without specific interaction terms being added.
- Importance vs. Significance: Feature importance in RF measures the overall contribution to predictive accuracy across potentially complex relationships. Statistical significance in linear regression tests a specific hypothesis about a linear relationship, holding other variables constant. A variable can be crucial for prediction in a non-linear or interactive way (high RF importance) even if its "linear" effect isn't necessarily significant.

So, what weather factors appear to influence vintage scores most significantly? Comparing all the findings from the various analyses conducted, this project suggests that variables related to visibility, wind speed, and max precipitation  are the most important weather-related factors affecting grape quality and, in turn, vintage scores. If I were to grow my own grapes to produce wine, I'd probably choose a location in the Northern Hemisphere where there is high year-round visibility, high average southernly winds, and less frequent but heavier rainfall. Asking different AI LLM models revealed that Phoenix, AZ (Perplexity), Antalya, Turkey (Gemini), and the Negev region in southern Israel (ChatGPT) would fit this criteria!

### Thanks for reading! 

