---
layout: page
title: Understanding Grocery Shopping Payments 2
permalink: /pages/grocery-part2
---

## Understanding Grocery Payments Part 2 

[Return to Part 1](/projects/grocery)

# Results

To evaluate how device type and payment methods impact consumer shopping behavior, I conducted some linear-mixed effects regression models on my three outcome variables: order cost, order quantity, and order variety. These regression models are similar to linear regression models in that they account for fixed effects (e.g. predictors like device type and payment method), but the main difference is that they also account for random effects (e.g. customer-level variation across multiple orders). Without going too much into the details, I'm employing this linear mixed effects model because the observations are not assumed to be independent so I will be able to better estimate the effects of the predictors. For all the models, I will control for the region of the customer, the month of the order, and the membership status.

<br>

#### Does device type and payment method affect (log-transformed) order costs?

<table style="border-collapse: collapse; width: 80%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; font-size: 14px; margin-bottom: 10px;">Linear Mixed Effects Model for Log Order Cost</caption>

  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>
  <tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
  <tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
  <tr><td style="text-align:left"></td><td>LogOrderCost</td></tr>
  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>

  <tr><td style="text-align:left">PaymentPayPal</td><td>0.033<sup>***</sup> (0.010)</td></tr>
  <tr><td style="text-align:left">PaymentPrepaid</td><td>0.154<sup>***</sup> (0.012)</td></tr>
  <tr><td style="text-align:left">PaymentWechat</td><td>0.274<sup>***</sup> (0.017)</td></tr>

  <tr><td style="text-align:left">RegionCH</td><td>-0.297<sup>***</sup> (0.043)</td></tr>
  <tr><td style="text-align:left">RegionDC</td><td>-0.198<sup>***</sup> (0.017)</td></tr>
  <tr><td style="text-align:left">RegionGA</td><td>-0.553<sup>***</sup> (0.026)</td></tr>
  <tr><td style="text-align:left">RegionIL</td><td>-0.459<sup>***</sup> (0.018)</td></tr>
  <tr><td style="text-align:left">RegionNJ</td><td>0.018 (0.018)</td></tr>
  <tr><td style="text-align:left">RegionNY</td><td>0.033<sup>**</sup> (0.014)</td></tr>
  <tr><td style="text-align:left">RegionON</td><td>-0.134<sup>***</sup> (0.022)</td></tr>

  <tr><td style="text-align:left">Month4</td><td>-0.151<sup>***</sup> (0.006)</td></tr>
  <tr><td style="text-align:left">Month5</td><td>-0.151<sup>***</sup> (0.006)</td></tr>

  <tr><td style="text-align:left">Membership1</td><td>-0.159<sup>***</sup> (0.014)</td></tr>

  <tr><td style="text-align:left">DeviceWeb</td><td>-0.059<sup>***</sup> (0.020)</td></tr>
  <tr><td style="text-align:left">DeviceiOS</td><td>0.081<sup>***</sup> (0.016)</td></tr>
  <tr><td style="text-align:left">DeviceiPad</td><td>0.137<sup>***</sup> (0.026)</td></tr>
  <tr><td style="text-align:left">DevicePC</td><td>0.242<sup>***</sup> (0.017)</td></tr>

  <tr><td style="text-align:left">Constant</td><td>4.338<sup>***</sup> (0.019)</td></tr>

  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>

  <tr><td style="text-align:left">Observations</td><td>110,584</td></tr>
  <tr><td style="text-align:left">Log Likelihood</td><td>-141,613.000</td></tr>
  <tr><td style="text-align:left">Akaike Inf. Crit.</td><td>283,266.100</td></tr>
  <tr><td style="text-align:left">Bayesian Inf. Crit.</td><td>283,458.300</td></tr>

  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>

  <tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>
<br>

The results from the regression table show that both device type and payment method affect customer spending. Starting with **payment method** (with credit card as the reference category), we see three terms that represent binary dummy variables for the remaining payment methods. 

<br>

The coefficient for PayPal is positive and significant (β = 0.032, SE = 0.010, p < 0.01), indicating that customers who pay with PayPal have an order cost that is 0.032 log units higher than customers who pay with credit card. To make this more interpretable, we can exponentiate the coefficient which returns the value as percent change. So a 0.032 increase in log units is equivalent to a 3.4% increase in order cost. Similarly, the coefficient for Wechat is positive and significant (β = 0.274, SE = 0.017, p < 0.001), suggesting that customers who pay with Wechat spend about 31.5% more than customers who pay with credit card. Finally, for prepaid, the coefficient is also positive and significant (β = 0.154, SE = 0.012, p < 0.001), meaning that on average, customers who pay with prepaid have an order cost that is 16.6% greater than customers who pay with credit card. 

<br>

For **device type** (with Android as the reference category), we see four terms that represent binary dummy variables for the remaining device types. The results reveal a negative and significant coefficient for the Web device (β = -0.059, SE = 0.020, p < 0.01), indicating that customers who use a Web browser device on average spend about 5.7% less than those who use an Android device. In contrast, the coefficient of iOS is positive and significant (β = 0.081, SE = 0.016, p < 0.001), meaning that customers who use iOS (iPhones) spend about 8.4% more on their orders than those who use an Android device. Similarly, the coefficient for iPad is positive and significant (β = 0.137, SE = 0.026, p < 0.001), suggesting that customers who use an iPad spend about 14.7% more than Android customers. Finally, the coefficient for PC is also positive and significant (β = 0.242, SE = 0.017, p < 0.001), which suggests that PC customers spend 27.3% more than Android customers.

<br>

Since the coefficients in the regression table reflect relative comparisons to the credit card payment method, it's also helpful to look at the model-estimated means for each payment method to get a better sense of the differences in spending.

<br>
<table style="border-collapse: collapse; width: 80%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; font-size: 14px; margin-bottom: 10px;">Model Estimated Means of Log Order Cost for Payment Method</caption>
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="border: 1px solid #ddd; padding: 8px;">Payment</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Estimated Mean</th>
      <th style="border: 1px solid #ddd; padding: 8px;">SE</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Estimated Mean ($)</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Asymp LCL</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Asymp UCL</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Z Ratio</th>
      <th style="border: 1px solid #ddd; padding: 8px;">P-value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">CreditCard</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.04</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.0109</td>
      <td style="border: 1px solid #ddd; padding: 8px;">$56.78</td> <!-- e^4.04 -->
      <td style="border: 1px solid #ddd; padding: 8px;">4.02</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.06</td>
      <td style="border: 1px solid #ddd; padding: 8px;">371.905</td>
      <td style="border: 1px solid #ddd; padding: 8px;">&lt;.0001</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">PayPal</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.07</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.0132</td>
      <td style="border: 1px solid #ddd; padding: 8px;">$58.51</td> <!-- e^4.07 -->
      <td style="border: 1px solid #ddd; padding: 8px;">4.05</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.10</td>
      <td style="border: 1px solid #ddd; padding: 8px;">307.686</td>
      <td style="border: 1px solid #ddd; padding: 8px;">&lt;.0001</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Prepaid</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.19</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.0147</td>
      <td style="border: 1px solid #ddd; padding: 8px;">$66.10</td> <!-- e^4.19 -->
      <td style="border: 1px solid #ddd; padding: 8px;">4.16</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.22</td>
      <td style="border: 1px solid #ddd; padding: 8px;">284.752</td>
      <td style="border: 1px solid #ddd; padding: 8px;">&lt;.0001</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Wechat</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.31</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.0193</td>
      <td style="border: 1px solid #ddd; padding: 8px;">$74.59</td> <!-- e^4.31 -->
      <td style="border: 1px solid #ddd; padding: 8px;">4.28</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.35</td>
      <td style="border: 1px solid #ddd; padding: 8px;">223.442</td>
      <td style="border: 1px solid #ddd; padding: 8px;">&lt;.0001</td>
    </tr>
  </tbody>
</table>
<br>

As you can see, the Wechat payment method has the highest predicted spending, followed by prepaid, PayPal, and then credit card. 

<br>
<table style="border-collapse: collapse; width: 80%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; font-size: 14px; margin-bottom: 10px;">Model Estimated Means of Log Order Cost for Device Type</caption>
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="border: 1px solid #ddd; padding: 8px;">Device</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Estimated Mean</th>
      <th style="border: 1px solid #ddd; padding: 8px;">SE</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Estimated Mean ($)</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Asymp LCL</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Asymp UCL</th>
      <th style="border: 1px solid #ddd; padding: 8px;">Z Ratio</th>
      <th style="border: 1px solid #ddd; padding: 8px;">P-value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Android</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.074</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.01703</td>
      <td style="border: 1px solid #ddd; padding: 8px;">$58.66</td> <!-- e^4.074 -->
      <td style="border: 1px solid #ddd; padding: 8px;">4.041</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.108</td>
      <td style="border: 1px solid #ddd; padding: 8px;">239.198</td>
      <td style="border: 1px solid #ddd; padding: 8px;">&lt;.0001</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Web</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.015</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.01709</td>
      <td style="border: 1px solid #ddd; padding: 8px;">$55.54</td> <!-- e^4.015 -->
      <td style="border: 1px solid #ddd; padding: 8px;">3.982</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.049</td>
      <td style="border: 1px solid #ddd; padding: 8px;">235.020</td>
      <td style="border: 1px solid #ddd; padding: 8px;">&lt;.0001</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">iOS</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.155</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.01031</td>
      <td style="border: 1px solid #ddd; padding: 8px;">$63.81</td> <!-- e^4.155 -->
      <td style="border: 1px solid #ddd; padding: 8px;">4.135</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.175</td>
      <td style="border: 1px solid #ddd; padding: 8px;">403.126</td>
      <td style="border: 1px solid #ddd; padding: 8px;">&lt;.0001</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">iPad</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.211</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.02367</td>
      <td style="border: 1px solid #ddd; padding: 8px;">$67.42</td> <!-- e^4.211 -->
      <td style="border: 1px solid #ddd; padding: 8px;">4.165</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.258</td>
      <td style="border: 1px solid #ddd; padding: 8px;">177.934</td>
      <td style="border: 1px solid #ddd; padding: 8px;">&lt;.0001</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">PC</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.316</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0.01335</td>
      <td style="border: 1px solid #ddd; padding: 8px;">$74.99</td> <!-- e^4.316 -->
      <td style="border: 1px solid #ddd; padding: 8px;">4.290</td>
      <td style="border: 1px solid #ddd; padding: 8px;">4.342</td>
      <td style="border: 1px solid #ddd; padding: 8px;">323.261</td>
      <td style="border: 1px solid #ddd; padding: 8px;">&lt;.0001</td>
    </tr>
  </tbody>
</table>
<br>

We see a similar pattern as the regression table where PC customers spend the most, followed by iPad/iOS users (iPad and iOS order costs did not significantly differ). Interestingly, the model predicts that Android customers actually spend slightly more than Web customers (this difference was found to be statistically significant: β = 0.059, SE = 0.020, p < 0.05).

<br>

**Why do PC Users spend more?**

As hypothesized, customers who shop for groceries using a laptop/PC spend more than those using other devices. To understand why, normally one would conduct experiments (or A/B tests as they are called) where the device type, PC vs smartphone, is manipulated. This approach would establish a causal relationship first and then one would test this causal relationship on other variables that reveal the underlying psychological processes. The current evidence is only correlational at best as there is no experimental manipulation. However, A/B tests are time-consuming and costly, so instead how about I propose two possible explanations.

  1. **Thinking vs Feeling Mindset**

  One theory is that shopping on a PC encourages a more deliberate, planned approach whereas shopping on a smartphone promotes more impulsive buying.   This could be because PCs are often associated with work and analytical thinking, prompting more thoughtful decision-making. Alternatively, it       could also be that there are two types of shoppers, PC shoppers and smartphone shoppers, and PC shoppers are inherently more deliberate than         smartphone shoppers. Without an experiment, its difficult to distinguish between these two possibilities but either way, deliberate thinking may     lead to larger grocery orders as customers think ahead and add everything they might need.

  2. **Larger Screen, More Visibility**

  A more obvious and simpler explanation (not as cool psychologically imo) is attention. We have all heard of the saying "out of sight, out of mind"   right? PCs have larger screens, allowing more products to be displayed. As a result, shoppers simply see more items, consider more items, and        purchase these items they may not have seen/thought about otherwise. This increase in exposure essentially leads to larger orders. 

<br>

**Paying by Wechat > Paying by Prepaid > Paying by Credit Card**

Unexpectedly, I found that customers spend the most on grocery orders when they pay using Wechat. As hypothesized, the results also showed that paying by a prepaid balance leads to a higher order cost than paying by credit card. As I discussed in the introduction, one might initially think that shoppers would spend the most paying by credit card. This is because the "pain of paying" is more abstract (you don't physically see the money going out of your hands) and it is decoupled from the consumption (you get to consume first and pay later). So what psychological mechanism could explain these discrepancies?

**Fungibility of Money**

Fungibility refers to the interchangeability of an asset, meaning that each unit is identical and can be substituted for another. In the case of  money, a \$20 bill is fungible because it can be exchanged for another \$20 bill, twenty \$1 bills, or other equivalent combinations. You can think of fungibility as substitutability.

<br>

So what does fungibility have to do with Wechat users spending the most on groceries? The answer lies in the concept of [mental accounting](https://www.jstor.org/stable/183904), a theory proposed by Nobel-winning behavioral economist Richard Thaler. Mental accounting refers to how people categorize money into separate "accounts" based on its source or purpose. For example, even though all income comes from the same source, people mentally separate it into rent, savings, and spendings. Likewise, money from different sources (i.e. paycheck vs. gift) is often treated differently despite economically being the same. This mental separation means that in practice money is not always fungible. 

<br>

Now let's apply this logic to paying by Wechat. First, I will assume that payment methods are often "sticky" as people tend to stick with their preferred payment method over time. If someone regularly pays for their groceries using Wechat, they probably will continue to do so. Now the key insight is that money in a Wechat digital wallet is less fungible than money from the other payment methods, including PayPal, a U.S. digital wallet. Unlike U.S. dollars in PayPal or from a credit card, money in Wechat (Chinese yuan or RMB) is not easily usable in the U.S. as many vendors don't accept Wechat payments. As a result, customers in the U.S. who use Wechat to pay for groceries aren't likely to think about other spending opportunities.

<br>

Similarly, money in a prepaid balance may also be seen as *less fungible*. This is because the money loaded into a prepaid balance is more restricted (even if the restriction is one additional step of transfering the money out). Thus, customers who use a prepaid balance to buy groceries may mentally categorize the prepaid balance as solely "grocery money". As such, prepaid customers may feel less hesitation when it comes to adding extra items to their cart or upgrading to higher quality products since the money is already "locked" into groceries. 

<br>

On the other hand, payment methods like PayPal and credit cards are *more fungible*. Money in a PayPal account or from a credit card can be spent on a wide range of expenses. This means that every dollar spent on groceries is a dollar less that could be spent for other purchases. Therefore, customers who pay using PayPal or credit card face greater *perceived opportunity costs* when they think about buying groceries, leading to a smaller-sized shopping cart relatively speaking.

<br>

---

#### Does device type and payment method affect (log-transformed) order quantity?

<table style="border-collapse: collapse; width: 80%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; font-size: 14px; margin-bottom: 10px;">Linear Mixed Effects Model for Log Order Quantity</caption>

  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>
  <tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
  <tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
  <tr><td style="text-align:left"></td><td>LogBasketItems</td></tr>
  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>

  <tr><td style="text-align:left">PaymentPayPal</td><td>-0.006 (0.009)</td></tr>
  <tr><td style="text-align:left">PaymentPrepaid</td><td>0.148<sup>***</sup> (0.011)</td></tr>
  <tr><td style="text-align:left">PaymentWechat</td><td>0.175<sup>***</sup> (0.015)</td></tr>

  <tr><td style="text-align:left">RegionCH</td><td>-0.344<sup>***</sup> (0.038)</td></tr>
  <tr><td style="text-align:left">RegionDC</td><td>-0.246<sup>***</sup> (0.015)</td></tr>
  <tr><td style="text-align:left">RegionGA</td><td>-0.576<sup>***</sup> (0.023)</td></tr>
  <tr><td style="text-align:left">RegionIL</td><td>-0.414<sup>***</sup> (0.016)</td></tr>
  <tr><td style="text-align:left">RegionNJ</td><td>-0.060<sup>***</sup> (0.016)</td></tr>
  <tr><td style="text-align:left">RegionNY</td><td>0.040<sup>***</sup> (0.013)</td></tr>
  <tr><td style="text-align:left">RegionON</td><td>-0.109<sup>***</sup> (0.020)</td></tr>

  <tr><td style="text-align:left">Month4</td><td>-0.047<sup>***</sup> (0.006)</td></tr>
  <tr><td style="text-align:left">Month5</td><td>-0.072<sup>***</sup> (0.006)</td></tr>

  <tr><td style="text-align:left">Membership1</td><td>-0.130<sup>***</sup> (0.013)</td></tr>

  <tr><td style="text-align:left">DeviceWeb</td><td>-0.191<sup>***</sup> (0.018)</td></tr>
  <tr><td style="text-align:left">DeviceiOS</td><td>0.020 (0.014)</td></tr>
  <tr><td style="text-align:left">DeviceiPad</td><td>0.118<sup>***</sup> (0.024)</td></tr>
  <tr><td style="text-align:left">DevicePC</td><td>0.178<sup>***</sup> (0.016)</td></tr>

  <tr><td style="text-align:left">Constant</td><td>2.545<sup>***</sup> (0.017)</td></tr>

  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>

  <tr><td style="text-align:left">Observations</td><td>110,584</td></tr>
  <tr><td style="text-align:left">Log Likelihood</td><td>-129,378.800</td></tr>
  <tr><td style="text-align:left">Akaike Inf. Crit.</td><td>258,797.600</td></tr>
  <tr><td style="text-align:left">Bayesian Inf. Crit.</td><td>258,989.900</td></tr>

  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>

  <tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>
<br>

The regression results here mirror those from the above analysis on order costs, which is unsurprising given that order costs and quantity are usually highly correlated. Thus, I will skip over a detailed discussion of this linear mixed-effects model as the underlying mechansims driving these results are likely the same as those unfluencing order costs.

<br>

---

#### Does device type and payment method impact customers' order variety?

<table style="border-collapse: collapse; width: 80%; text-align: center; margin: 0 auto; font-size: 12px;">
  <caption style="font-weight: bold; font-size: 14px; margin-bottom: 10px;">Linear Mixed Effects Model for Order Variety</caption>

  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>
  <tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
  <tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
  <tr><td style="text-align:left"></td><td>BasketV</td></tr>
  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>

  <tr><td style="text-align:left">PaymentPayPal</td><td>-0.043<sup>***</sup> (0.015)</td></tr>
  <tr><td style="text-align:left">PaymentPrepaid</td><td>0.206<sup>***</sup> (0.017)</td></tr>
  <tr><td style="text-align:left">PaymentWechat</td><td>0.144<sup>***</sup> (0.024)</td></tr>

  <tr><td style="text-align:left">RegionCH</td><td>-0.587<sup>***</sup> (0.061)</td></tr>
  <tr><td style="text-align:left">RegionDC</td><td>-0.359<sup>***</sup> (0.025)</td></tr>
  <tr><td style="text-align:left">RegionGA</td><td>-0.612<sup>***</sup> (0.037)</td></tr>
  <tr><td style="text-align:left">RegionIL</td><td>-0.595<sup>***</sup> (0.026)</td></tr>
  <tr><td style="text-align:left">RegionNJ</td><td>-0.166<sup>***</sup> (0.026)</td></tr>
  <tr><td style="text-align:left">RegionNY</td><td>0.160<sup>***</sup> (0.020)</td></tr>
  <tr><td style="text-align:left">RegionON</td><td>-0.033 (0.032)</td></tr>

  <tr><td style="text-align:left">Month4</td><td>-0.047<sup>***</sup> (0.009)</td></tr>
  <tr><td style="text-align:left">Month5</td><td>-0.095<sup>***</sup> (0.009)</td></tr>

  <tr><td style="text-align:left">Membership1</td><td>-0.054<sup>***</sup> (0.020)</td></tr>

  <tr><td style="text-align:left">DeviceWeb</td><td>-0.234<sup>***</sup> (0.028)</td></tr>
  <tr><td style="text-align:left">DeviceiOS</td><td>0.027 (0.022)</td></tr>
  <tr><td style="text-align:left">DeviceiPad</td><td>0.076<sup>**</sup> (0.037)</td></tr>
  <tr><td style="text-align:left">DevicePC</td><td>0.128<sup>***</sup> (0.025)</td></tr>

  <tr><td style="text-align:left">Constant</td><td>3.395<sup>***</sup> (0.027)</td></tr>

  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>

  <tr><td style="text-align:left">Observations</td><td>110,584</td></tr>
  <tr><td style="text-align:left">Log Likelihood</td><td>-179,222.000</td></tr>
  <tr><td style="text-align:left">Akaike Inf. Crit.</td><td>358,483.900</td></tr>
  <tr><td style="text-align:left">Bayesian Inf. Crit.</td><td>358,676.200</td></tr>

  <tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr>

  <tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>
<br>

Once again, a similar pattern emerges. In terms of device type, PC users demonstrated the highest order variety while Wechat and prepaid buyers displayed the highest order variety (no significant difference between Wechat and prepaid payment methods on order variety). This is consistent with the idea that higher spending tends to be associated with purchasing more items and a greater varitey of items.

# Conclusion

This project revealed several fascinating insights about online grocery shopping behavior that I did not know when I started this project. One of the most notable findings is that merely the option to pay can affect how much consumers spend on groceries. Additionally, the device consumers use to shop online may also impact what they spend on. 

<br>

Part of my training in a marketing PhD was to think about implications and I think these results should inspire some serious thought about online shopping in general.
1. Leverage Non-Fungible Money: Retailers should incorporate prepaid balances and offer discounts or promotions to load funds into prepaid balances (e.g. Buy \$100 get \$5 extra). This not only helps retailers earn cash at the beginning of the customer acquisition process but also it can encourage consumers to stay on long-term.
2. Promote Deliberative Thinking: While impulsive shopping is often seen as a driver of higher spending, these results counterintuitively suggest that deliberative shopping may lead to larger purchases. Its likely that the spending from deliberative shopping may outweigh that of impulsive shopping because impulsive purchases may be smaller ticket items. Encouraging customers to deliberate may lead them to increase purchases especially if product benefits or suggestions are emphasized.

<br>

I hope you learned something about consumer behavior in grocery shopping! Please reach out to me if you have any ideas, questions, or comments -- I would love to hear and discuss them all.

### Thanks for reading!

