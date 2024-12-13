---
layout: page
title: Understanding Grocery Shopping Payments 2
permalink: /pages/grocery-part2
---

## Understanding Grocery Payments Part 2 

[Return to Part 1](/projects/grocery)

<br>

# Results

To evaluate how device type and payment methods impact consumer shopping behavior, I conducted some linear-mixed effects regression models on my three outcome variables: order cost, order quantity, and order variety. These regression models are similar to linear regression models in that they account for fixed effects (e.g. predictors like device type and payment method), but the main difference is that they also account for random effects (e.g. customer-level variation across multiple orders). Without going too much into the details, I'm employing this linear mixed effects model because the observations are not assumed to be independent so I will be able to better estimate the effects of the predictors. For all the models, I will control for the region of the customer, the month of the order, and the membership status.

<br>

Does device type and payment method impact customers' (log-transformed) order costs?

<br>

<table style="text-align:center"><caption><strong>Linear Mixed Effects Model for Log Order Cost</strong></caption>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>LogOrderCost</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">PaymentPayPal</td><td>0.033<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.010)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">PaymentPrepaid</td><td>0.154<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.012)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">PaymentWechat</td><td>0.274<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.017)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">RegionCH</td><td>-0.297<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.043)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">RegionDC</td><td>-0.198<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.017)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">RegionGA</td><td>-0.553<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.026)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">RegionIL</td><td>-0.459<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.018)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">RegionNJ</td><td>0.018</td></tr>
<tr><td style="text-align:left"></td><td>(0.018)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">RegionNY</td><td>0.033<sup>**</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.014)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">RegionON</td><td>-0.134<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.022)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">Month4</td><td>-0.151<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.006)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">Month5</td><td>-0.151<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.006)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">Membership1</td><td>-0.159<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.014)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">DeviceWeb</td><td>-0.059<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.020)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">DeviceiOS</td><td>0.081<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.016)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">DeviceiPad</td><td>0.137<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.026)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">DevicePC</td><td>0.242<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.017)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>4.338<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.019)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>110,584</td></tr>
<tr><td style="text-align:left">Log Likelihood</td><td>-141,613.000</td></tr>
<tr><td style="text-align:left">Akaike Inf. Crit.</td><td>283,266.100</td></tr>
<tr><td style="text-align:left">Bayesian Inf. Crit.</td><td>283,458.300</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

---

<br>

Does device type and payment method impact customers' order quantity (log-transformed order quantity)

<br>

---

<br>