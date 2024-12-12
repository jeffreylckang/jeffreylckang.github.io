---
layout: page
title: Understanding Grocery Shopping Payments
permalink: /projects/grocery
editor_options:
  markdown:
    mode: gfm
---

## Understanding Grocery Shopping Payments

[Skip to Part 2](/pages/grocery-part2)

<br>

A few years ago, I was very fortunate enough to obtain transaction-level online grocery store data from a family friend. At the time, I was pursuing my PhD in Marketing and saw this as a perfect opportunity for a research paper. Naturally, I was curious about consumer shopping patterns, particularly in online grocery shopping, which saw significant growth during the Covid-19 pandemic.

<br>

Given the nature of this data, I wanted to explore how different factors influence consumer behavior. For instance, many consumers own both a smartphone and a laptop/PC giving them the flexibility to choose which device to use for purchases. Does the device type impact spending amount or the products purchased? Anecdotally, making purchases on a phone feels more spontaneous compared to a laptop/PC, which in the context of groceries might lead to more "vice" purchases.

<br>

Another factor to consider in online grocery shopping is the payment method. Virtually every online store offers multiple payment options at checkout: customers can typically pay using a credit card, prepaid card, or digital wallet. Could these payment methods affect spending behavior and the types of products purchased? Existing research in consumer psychology suggests it could, driven by two mechanisms: [payment transparency](https://link.springer.com/article/10.1023/A:1027444717586) and [payment coupling](https://pubsonline.informs.org/doi/10.1287/mksc.17.1.4).

-   Payment transparency refers to the visibility of the payment's form and amount. When payment is more transparent (like using cash), people experience a greater "pain of paying", which usually discourages spending. Credit cards, by contrast, have lower transparency since the payment is more abstract.

<!-- -->

-   Payment coupling refers to the temporal link between payment and consumption. With cash, the payment and consumption are tightly linked, but with credit cards, the payment is decoupled—delayed until the credit card bill arrives at the end of each month. This delay reduces the "pain of paying", which can encourage spending.

In theory, one might predict that since credit cards are less transparent and decoupled, customers may spend more when using credit cards compared to other forms of payment. As a result, I should expect that online grocery orders paid with credit cards will have a higher bill than other payment methods.

<br>

Let's analyze the data and see if these hypotheses hold true! This project will be organized as follows: I'll begin by describing and exploring the data, followed by a discussion of the results and takeaways.

# Data

I obtained two transaction-level sales datasets. The first dataset contains 591,140 orders made by 57,077 unique customers for the year 2021. Each observation in this dataset contains a customer's ID, their order number, the order date, the delivery date, the status of delivery, the total cost of the order, the payment method, the geographical region in the U.S., and the membership status. The second dataset contains 1,765,641 products made by 31,778 unique customers from January to May of 2021. Each observation in the second dataset contains a product purchased by a customer, the customer ID, the associated order number, the order date, the delivery date, the quantity of the product purchased, the price of the product, the product category, and the U.S. geographical region.

<br>

I merged the two datasets by matching customer IDs and order numbers to create a dataset at the order or "shopping cart" level. To do this, I aggregated the columns of each order from the first dataset, allowing me to consolidate each customer's shopping cart for the same variables present in the order-level dataset. I then excluded orders in which the delivery status was not completed (as some orders were refunded or canceled). Furthermore, I excluded orders of which the shopping cart cost was less than \$1 because such transactions aren't very meaningful in this analysis (I don't have a good explanation as to why there are transactions less than \$2 in the data but my best guess is that the company had some filler charges to customers?). As a result, the final dataset for analysis contains 109,010 orders made by 29,592 unique customers. Below is a table that reports some summary statistics of the data. <br>

<div style="text-align: center; margin: 0px 0;">
  <h3 style="margin: 0; font-size: 14px;">Descriptive Statistics of Transaction Data Set</h3>
</div>

|                                                           |         |
|-----------------------------------------------------------|---------|
| Number of Orders                                          | 109,010 |
| Number of Unique Customers                                | 29,592  |
| Average Order Cost                                        | 80.59   |
| SD Order Cost                                             | 67.63   |
| Min Order Cost                                            | 1.01    |
| Max Order Cost                                            | 1799.42 |
| Average Number of Items in Order                          | 13      |
| Average Variety of Product Categories (out of 6) in Order | 3       |

::: {style="text-align: center;"} <img src="https://jeffreylckang.github.io/assets/img/projects/grocery/Payment_Method_Distribution.png" alt="Payment_Method_Distribution" width="600"/> :::

Reason 1: Outliers / Heavy Users

```         
•   In Summary 1, every transaction is included in the mean. If a few users made very large purchases using Wechat, those large transactions inflate the overall average for Wechat.
•   However, in Summary 2, since we are averaging user-level means, those high-spending users don’t have as much of an impact.
•   Example:
•   Suppose one Wechat user spends $1000 and 10 other users spend $10.
•   The overall average (Summary 1) would be inflated by that large spender.
•   But in Summary 2, each user’s spend is treated equally, so that big spender is “flattened” by the larger pool of users.

Reason 2: Differences in User Behavior

•   Some users may only make 1 or 2 large purchases on Wechat, while other users make many smaller purchases using Credit Card or PayPal.
•   Since Summary 2 treats each user equally, even if one user spends $1000 once on Wechat and another user spends $10 in 100 transactions, they would be treated the same in this view.
•   Example:
•   User A (Wechat) spends $1000 in 1 transaction (Avg = $1000)
•   User B (Wechat) spends $50, $20, and $30 in 3 transactions (Avg = $33.33)
•   In Summary 1, all of these transactions are pooled, and the overall average is high.
•   In Summary 2, the user averages are $1000 and $33.33, so the final “average user spend” will be closer to the two users’ personal averages rather than their raw transactions.

Reason 3: Simpson’s Paradox (The Aggregation Effect)
```

This is a great example of the Simpson’s Paradox, where aggregating data at a “higher level” gives you one result, but looking at sub-grouped user-level data gives you another result.

In this case: • Summary 1 is heavily affected by users who spend a lot in just one payment method (like Wechat). • Summary 2 is more of a “user-centered view” — it treats every user as equally important. • When you aggregate to users, heavy spenders are “flattened” by all the other users with low or average spend.

```         
Remember though for descriptive stats I am looking at regular price not log price
```
