---
layout: page
title: Understanding Grocery Shopping Payments
permalink: /projects/grocery
---

## Understanding Grocery Shopping Payments

[Skip to Part 2](/pages/grocery-part2)

<br>

A few years ago, I was very fortunate enough to obtain transaction-level online grocery store data from a family friend who is employed at an Asian online grocery delivery company that services mainly the East Coast of the U.S. At the time, I was pursuing my PhD in Marketing and saw this as a perfect opportunity for a research paper. Naturally, I was curious about consumer shopping patterns, particularly in online grocery shopping, which saw significant growth during the Covid-19 pandemic.

# Introduction

Given the nature of this data, I wanted to explore how different factors influence consumer behavior. For instance, many consumers own both a smartphone and a laptop/PC giving them the flexibility to choose which device to use for purchases. Does the device type impact spending amount or the products purchased? Anecdotally, making purchases on a phone feels more spontaneous compared to a laptop/PC, which in the context of groceries might lead to more smaller purchases.

<br>

Another factor to consider in online grocery shopping is the payment method. Virtually every online store offers multiple payment options at checkout: customers can typically pay using a credit card, prepaid card, or digital wallet. Could these payment methods affect spending behavior and the types of products purchased? Existing research in consumer psychology suggests it could, driven by two mechanisms: [payment transparency](https://link.springer.com/article/10.1023/A:1027444717586) and [payment coupling](https://pubsonline.informs.org/doi/10.1287/mksc.17.1.4).

-   Payment transparency refers to the visibility of the payment's form and amount. When payment is more transparent (like using cash), people experience a greater "pain of paying", which usually discourages spending. Credit cards, by contrast, have lower transparency since the payment is more abstract.

<!-- -->

-   Payment coupling refers to the temporal link between payment and consumption. With cash, the payment and consumption are tightly linked, but with credit cards, the payment is decoupled—delayed until the credit card bill arrives at the end of each month. This delay reduces the "pain of paying", which can encourage spending.

In theory, one might predict that since credit cards are less transparent and decoupled, customers may spend more when using credit cards compared to other forms of payment. As a result, I should expect that online grocery orders paid with credit cards will have a higher bill than other payment methods.

<br>

Let's analyze the data and see if these hypotheses hold true! This project will be organized as follows: I'll begin by describing and exploring the data, followed by a discussion of the results and takeaways.

# Data

I obtained two transaction-level sales datasets. The first dataset contains 591,140 orders made by 57,077 unique customers for the year 2021. Each observation in this dataset contains a customer's ID, their order number, the order date, the delivery date, the status of delivery, the total cost of the order, the payment method, the geographical region in the U.S., and the membership status. The second dataset contains 1,765,641 products made by 31,778 unique customers from the months February, April, and May of 2021. Each observation in the second dataset contains a product purchased by a customer, the customer ID, the associated order number, the order date, the delivery date, the quantity of the product purchased, the price of the product, the product category, and the U.S. geographical region.

<br>

I merged the two datasets by matching customer IDs and order numbers to create a dataset at the order or "shopping cart" level. To do this, I aggregated the columns of each order from the first dataset, allowing me to consolidate each customer's shopping cart for the same variables present in the order-level dataset. I then excluded orders in which the delivery status was not completed (as some orders were refunded or canceled). Furthermore, I excluded orders of which the shopping cart cost was less than \$1 because such transactions aren't very meaningful in this analysis (I don't have a good explanation as to why there are transactions less than \$2 in the data but my best guess is that the company had some filler charges to customers?). As a result, the final dataset for analysis contains 109,010 orders made by 29,592 unique customers. Below is a table that reports some summary statistics of the data.

<br>

<div style="text-align: center;">
  <table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
    <caption style="font-weight: bold; margin-bottom: 10px;">Descriptive Statistics of Transaction Data</caption>
    <tbody>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Number of Orders</td>
        <td style="border: 1px solid #ddd; padding: 8px;">109,010</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Number of Unique Customers</td>
        <td style="border: 1px solid #ddd; padding: 8px;">29,592</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Average Order Cost ($)</td>
        <td style="border: 1px solid #ddd; padding: 8px;">80.59</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">SD Order Cost ($)</td>
        <td style="border: 1px solid #ddd; padding: 8px;">67.63</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Min Order Cost ($)</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.01</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Max Order Cost ($)</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1799.42</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Average Number of Items in Order</td>
        <td style="border: 1px solid #ddd; padding: 8px;">13</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Average Variety of Product Categories (out of 6) in Order</td>
        <td style="border: 1px solid #ddd; padding: 8px;">3</td>
      </tr>
    </tbody>
  </table>
</div>

#### Exploring the Data

Since I'm interested in device type and payment method, I'll examine how these factors affect key outcomes like spending amount, purchase quantity, and purchase variety. These outcomes either weren't accurate or directly included in the dataset, so I engineered them as new features. **Spending amount or Order cost** was calculated as the quantity * price for each product for each order in the dataset. **Order quantity** was calculated as the total number of products in each order. **Order variety** was measured as the count of distinct product categories in the cart, with a maximum of 6 possible categories. 

<br>

Let's take a quick look at each outcome variable. We'll start with the order cost. We've already seen the average order cost from the table above, so I'll plot the distribution of the order cost.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/grocery/basketcostimg.png" alt="Distribution of Basket Cost" width="600" /> </div>

<br>

We can see that the distribution is not normal, so let's apply a log transformation on this variable.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/grocery/logbasketcostimg.png" alt="Distribution of Log Basket Cost" width="600" /> </div>

<br>

It seems like the log transformation made it more normally distributed. We'll keep that for now. Let's move onto the purchase quantity. I'll also plot the distribution below.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/grocery/basketitemsimg.png" alt="Distribution of Basket Quantity" width="600" /> </div>

<br>

The distribution is heavily skewed with a long right tail. We can also try applying a log transformation to see if it helps normalize the data.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/grocery/logbasketitemsimg.png" alt="Distribution of Log Basket Quantity" width="600" /> </div>

<br>

The log transformation seems to help normalize the distribution a bit, but it doesn't seem great. We'll also keep this for now. Finally, let's look at the purchase variety.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/grocery/basketvimg.png" alt="Distribution of Basket Variety" width="600" /> </div>

<br>

While this distribution looks a bit more normal, notice that peaks at each unit. This is because purchase variety has a range from one to six and it could be considered a multi-class categorical variable. For analysis purposes, I'll treat it as a continuous variable since the analysis will be easier to interpret.

<br>

---

Having examined the outcome variables, let's dive into some of the more exciting aspects of the data. First, I'll explore the proportion of **device types** used by the customers in the data.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/grocery/Device_Distribution.png" alt="Device Type Frequency" width="600" /> </div>

<br>

We can see from the pie chart that most shoppers in the data are buying groceries using their iPhones. Maybe that isn't as surprising even in 2021. I should note here that the device type categories were defined by the data provider, so I have no control over their names or classifications. If you are wondering what the difference is between Web and laptop/PC, I had exactly the same question. My best guess is that the "Web" category represents shoppers who use a web browser to buy groceries that the company couldn't classify into iOS or PC. Now that we know the distribution, how might this affect spending? One quick note: because the log-transformed order cost is harder to interpret, I'll present the subsequent results in raw units.

<br>

<div style="text-align: center;"> 
  <table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
    <caption style="font-weight: bold; margin-bottom: 10px;">Average Order Cost by Device</caption>
    <thead>
      <tr style="background-color: #f2f2f2;">
        <th style="border: 1px solid #ddd; padding: 8px;">Device</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Mean ($)</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD ($)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Android</td>
        <td style="border: 1px solid #ddd; padding: 8px;">69.60</td>
        <td style="border: 1px solid #ddd; padding: 8px;">62.99</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Web</td>
        <td style="border: 1px solid #ddd; padding: 8px;">51.39</td>
        <td style="border: 1px solid #ddd; padding: 8px;">56.96</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">iOS</td>
        <td style="border: 1px solid #ddd; padding: 8px;">78.96</td>
        <td style="border: 1px solid #ddd; padding: 8px;">66.72</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">iPad</td>
        <td style="border: 1px solid #ddd; padding: 8px;">82.74</td>
        <td style="border: 1px solid #ddd; padding: 8px;">70.67</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">PC</td>
        <td style="border: 1px solid #ddd; padding: 8px;">101.73</td>
        <td style="border: 1px solid #ddd; padding: 8px;">70.47</td>
      </tr>
    </tbody>
  </table>
</div>

<br>

On average, customers who shop for groceries on their PC tend to spend the most. However, the interpretation of the average spending by device can be somewhat misleading because this method assumes each order as if it came from a unique user. To get a clearer picture, we should also calculate each user's average spending by device and then aggregate these user-level averages. This approach accounts for individual differences in spending habits across devices.

<br>
<div style="text-align: center;"> 
  <table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
    <caption style="font-weight: bold; margin-bottom: 10px;">Average User Order Cost by Device</caption>
    <thead>
      <tr style="background-color: #f2f2f2;">
        <th style="border: 1px solid #ddd; padding: 8px;">Device</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Avg Spend Per User ($)</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD Spend Per User ($)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Android</td>
        <td style="border: 1px solid #ddd; padding: 8px;">91.02</td>
        <td style="border: 1px solid #ddd; padding: 8px;">58.89</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Web</td>
        <td style="border: 1px solid #ddd; padding: 8px;">67.52</td>
        <td style="border: 1px solid #ddd; padding: 8px;">57.64</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">iOS</td>
        <td style="border: 1px solid #ddd; padding: 8px;">96.30</td>
        <td style="border: 1px solid #ddd; padding: 8px;">59.70</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">iPad</td>
        <td style="border: 1px solid #ddd; padding: 8px;">103.85</td>
        <td style="border: 1px solid #ddd; padding: 8px;">63.93</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">PC</td>
        <td style="border: 1px solid #ddd; padding: 8px;">107.34</td>
        <td style="border: 1px solid #ddd; padding: 8px;">61.59</td>
      </tr>
    </tbody>
  </table>
</div>
<br>

There doesn't appear to be much of a difference in relational terms between the different device types for order cost. Next, let's examine purchase quantity by device type using a similar approach. 

<br>

<div style="text-align: center;"> 
  <table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
    <caption style="font-weight: bold; margin-bottom: 10px;">Average Order Quantity By Device</caption>
    <thead>
      <tr style="background-color: #f2f2f2;">
        <th style="border: 1px solid #ddd; padding: 8px;">Device</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Avg Quantity</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD Quantity</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Android</td>
        <td style="border: 1px solid #ddd; padding: 8px;">11.91</td>
        <td style="border: 1px solid #ddd; padding: 8px;">10.60</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Web</td>
        <td style="border: 1px solid #ddd; padding: 8px;">8.43</td>
        <td style="border: 1px solid #ddd; padding: 8px;">9.31</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">iOS</td>
        <td style="border: 1px solid #ddd; padding: 8px;">12.80</td>
        <td style="border: 1px solid #ddd; padding: 8px;">10.64</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">iPad</td>
        <td style="border: 1px solid #ddd; padding: 8px;">13.86</td>
        <td style="border: 1px solid #ddd; padding: 8px;">11.30</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">PC</td>
        <td style="border: 1px solid #ddd; padding: 8px;">16.94</td>
        <td style="border: 1px solid #ddd; padding: 8px;">11.95</td>
      </tr>
    </tbody>
  </table>
</div>

<br>

<div style="text-align: center;"> 
  <table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
    <caption style="font-weight: bold; margin-bottom: 10px;">Average User Order Quantity By Device</caption>
    <thead>
      <tr style="background-color: #f2f2f2;">
        <th style="border: 1px solid #ddd; padding: 8px;">Device</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Avg Items</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD Items</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Android</td>
        <td style="border: 1px solid #ddd; padding: 8px;">15.47</td>
        <td style="border: 1px solid #ddd; padding: 8px;">10.33</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Web</td>
        <td style="border: 1px solid #ddd; padding: 8px;">10.77</td>
        <td style="border: 1px solid #ddd; padding: 8px;">9.48</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">iOS</td>
        <td style="border: 1px solid #ddd; padding: 8px;">15.39</td>
        <td style="border: 1px solid #ddd; padding: 8px;">9.88</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">iPad</td>
        <td style="border: 1px solid #ddd; padding: 8px;">17.40</td>
        <td style="border: 1px solid #ddd; padding: 8px;">10.75</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">PC</td>
        <td style="border: 1px solid #ddd; padding: 8px;">17.88</td>
        <td style="border: 1px solid #ddd; padding: 8px;">11.14</td>
      </tr>
    </tbody>
  </table>
</div>

<br>

Customers purchase the most items using a PC than the other devices which is unsurprising given that purchase amount and quantity are often highly correlated. Finally, let's examine order variety. 

<br>

<div style="text-align: center;"> 
  <table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
    <caption style="font-weight: bold; margin-bottom: 10px;">Average Order Variety By Device</caption>
    <thead>
      <tr style="background-color: #f2f2f2;">
        <th style="border: 1px solid #ddd; padding: 8px;">Device</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Avg Variety</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD Variety</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Android</td>
        <td style="border: 1px solid #ddd; padding: 8px;">2.90</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.43</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Web</td>
        <td style="border: 1px solid #ddd; padding: 8px;">2.34</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.36</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">iOS</td>
        <td style="border: 1px solid #ddd; padding: 8px;">3.02</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.43</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">iPad</td>
        <td style="border: 1px solid #ddd; padding: 8px;">3.05</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.44</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">PC</td>
        <td style="border: 1px solid #ddd; padding: 8px;">3.40</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.39</td>
      </tr>
    </tbody>
  </table>
</div>

<br>

Order variety appears to be equal amongst the different devices. I won't present the average user order variety results since they are quite similar to the table above.

<br> 

---

Next, let's examine the proportion of **payment methods** used by the customers in the data.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/grocery/Payment_Method_Distribution2.png" alt="Device Type Frequency" width="600" /> </div>

<br>

We can see that most shoppers pay by credit card, which maybe says something about it still being the most convenient form of payment. I should also note for those unfamiliar with "Wechat" that it is a social networking app with an integrated digital wallet. Keeping this in mind, let's look at the average order cost and average user order cost.

<br>

<div style="text-align: center;"> 
  <table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
    <caption style="font-weight: bold; margin-bottom: 10px;">Average Order Cost by Payment Method</caption>
    <thead>
      <tr style="background-color: #f2f2f2;">
        <th style="border: 1px solid #ddd; padding: 8px;">Payment Method</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Avg Order Cost ($)</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD Order Cost ($)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">CreditCard</td>
        <td style="border: 1px solid #ddd; padding: 8px;">80.37</td>
        <td style="border: 1px solid #ddd; padding: 8px;">66.22</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">PayPal</td>
        <td style="border: 1px solid #ddd; padding: 8px;">84.10</td>
        <td style="border: 1px solid #ddd; padding: 8px;">65.06</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Prepaid</td>
        <td style="border: 1px solid #ddd; padding: 8px;">67.74</td>
        <td style="border: 1px solid #ddd; padding: 8px;">73.94</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Wechat</td>
        <td style="border: 1px solid #ddd; padding: 8px;">106.42</td>
        <td style="border: 1px solid #ddd; padding: 8px;">71.62</td>
      </tr>
    </tbody>
  </table>
</div>

<br>

<div style="text-align: center;"> 
  <table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
    <caption style="font-weight: bold; margin-bottom: 10px;">Average User Order Cost by Payment Method</caption>
    <thead>
      <tr style="background-color: #f2f2f2;">
        <th style="border: 1px solid #ddd; padding: 8px;">Payment Method</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Avg User Order Cost ($)</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD User Order Cost ($)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">CreditCard</td>
        <td style="border: 1px solid #ddd; padding: 8px;">97.06</td>
        <td style="border: 1px solid #ddd; padding: 8px;">59.01</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">PayPal</td>
        <td style="border: 1px solid #ddd; padding: 8px;">94.98</td>
        <td style="border: 1px solid #ddd; padding: 8px;">57.33</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Prepaid</td>
        <td style="border: 1px solid #ddd; padding: 8px;">97.20</td>
        <td style="border: 1px solid #ddd; padding: 8px;">71.06</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Wechat</td>
        <td style="border: 1px solid #ddd; padding: 8px;">104.79</td>
        <td style="border: 1px solid #ddd; padding: 8px;">66.77</td>
      </tr>
    </tbody>
  </table>
</div>

<br>

Looking at both tables, we can first identify that Wechat payment has the highest average spending per order, followed by that of credit cards, and then prepaid balances. However, at the user-level, Wechat payment still remains the highest but the average spending by prepaid balances is equal to that of credit cards. This shift seems confusing. Why might this be the case?

<br>

Well, a really cool explanation of this could be the [Simpson's Paradox or the Aggregation Effect](https://en.wikipedia.org/wiki/Simpson%27s_paradox#:~:text=Simpson's%20paradox%20is%20a%20phenomenon,when%20the%20groups%20are%20combined.). This refers to a phenomenon in which aggregating data ta a higher level shows one result, but aggregating at a sub-level shows another. In this case, certain customers may spend a lot using credit cards, inflating the overall average. But when we calculate user-level averages, these high-cost orders are "flattened" by other customers with lower spending on credit card orders (remember credit card payments represent the highest proportion of payments). In contrast, prepaid balance customers may have fewer total orders, but their spending per order is more consistent, leading to a higher user-level average.

<br>

For order quantity and order variety, I'll focus on user-level results rather than order-level averages. Since payment methods for groceries tend to be habitual, user-level differences should provide a more meaningful interpretation.

<div style="text-align: center;"> 
  <table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
    <caption style="font-weight: bold; margin-bottom: 10px;">Average User Order Quantity by Payment Method</caption>
    <thead>
      <tr style="background-color: #f2f2f2;">
        <th style="border: 1px solid #ddd; padding: 8px;">Payment Method</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Avg User Order Quantity</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD User Order Quantity</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">CreditCard</td>
        <td style="border: 1px solid #ddd; padding: 8px;">15.90</td>
        <td style="border: 1px solid #ddd; padding: 8px;">10.20</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">PayPal</td>
        <td style="border: 1px solid #ddd; padding: 8px;">15.13</td>
        <td style="border: 1px solid #ddd; padding: 8px;">9.79</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Prepaid</td>
        <td style="border: 1px solid #ddd; padding: 8px;">15.82</td>
        <td style="border: 1px solid #ddd; padding: 8px;">11.40</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Wechat</td>
        <td style="border: 1px solid #ddd; padding: 8px;">16.01</td>
        <td style="border: 1px solid #ddd; padding: 8px;">10.18</td>
      </tr>
    </tbody>
  </table>
</div>

<br> 

<div style="text-align: center;"> 
  <table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
    <caption style="font-weight: bold; margin-bottom: 10px;">Average User Order Variety by Payment Method</caption>
    <thead>
      <tr style="background-color: #f2f2f2;">
        <th style="border: 1px solid #ddd; padding: 8px;">Payment Method</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Avg User Order Variety</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD User Order Variety</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">CreditCard</td>
        <td style="border: 1px solid #ddd; padding: 8px;">3.34</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.25</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">PayPal</td>
        <td style="border: 1px solid #ddd; padding: 8px;">3.24</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.27</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Prepaid</td>
        <td style="border: 1px solid #ddd; padding: 8px;">3.35</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.20</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Wechat</td>
        <td style="border: 1px solid #ddd; padding: 8px;">3.39</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.29</td>
      </tr>
    </tbody>
  </table>
</div>

<br>

As expected, Wechat payment customers seem to purchase the most quantity and variety compared to the other payment methods.

<br>

---

Let's quickly look at the proportion of **customer membership**.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/grocery/Membership_Distribution.png" alt="Membership Frequency" width="600" /> </div>

<br>

Most customers in the data set appear to be non-members, which isn't really surprising. We'll look at some descriptive means of the three outcome variables below. 

<br>

<div style="text-align: center;"> 
  <table style="border-collapse: collapse; width: 70%; text-align: center; margin: 0 auto; font-size: 12px;">
    <caption style="font-weight: bold; margin-bottom: 10px;">Average Order Cost, Quantity, and Variety by Membership Status</caption>
    <thead>
      <tr style="background-color: #f2f2f2;">
        <th style="border: 1px solid #ddd; padding: 8px;">Membership Status</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Avg Order Cost</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD Order Cost</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Avg Order Quantity</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD Order Quantity</th>
        <th style="border: 1px solid #ddd; padding: 8px;">Avg Order Variety</th>
        <th style="border: 1px solid #ddd; padding: 8px;">SD Order Variety</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Non-member</td>
        <td style="border: 1px solid #ddd; padding: 8px;">83.54</td>
        <td style="border: 1px solid #ddd; padding: 8px;">65.58</td>
        <td style="border: 1px solid #ddd; padding: 8px;">13.75</td>
        <td style="border: 1px solid #ddd; padding: 8px;">10.90</td>
        <td style="border: 1px solid #ddd; padding: 8px;">3.10</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.44</td>
      </tr>
      <tr>
        <td style="border: 1px solid #ddd; padding: 8px;">Member</td>
        <td style="border: 1px solid #ddd; padding: 8px;">70.31</td>
        <td style="border: 1px solid #ddd; padding: 8px;">73.41</td>
        <td style="border: 1px solid #ddd; padding: 8px;">11.34</td>
        <td style="border: 1px solid #ddd; padding: 8px;">11.08</td>
        <td style="border: 1px solid #ddd; padding: 8px;">2.85</td>
        <td style="border: 1px solid #ddd; padding: 8px;">1.42</td>
      </tr>
    </tbody>
  </table>
</div>

<br>

The means demonstrate that non-members spend more, purchase more items, and have greater product variety than members. This was unexpected, but it could be due to the larger number of non-members compared to members in the data set. I wonder if this pattern holds true for other online grocery stores or even for retailers (e.g. Amazon, BestBuy, Nike, etc...)?

<br>

---

Last but not least, let's examine the distribution of **region**.

<br>

<div style="text-align: center;"> <img src="https://jeffreylckang.github.io/assets/img/projects/grocery/Region_Distribution.png" alt="Region Frequency" width="600" /> </div>

<br>

As mentioned before, the data comes from an online grocery store that services mainly the East Coast region of the U.S. As you can see, the majority of its customers are from the East Coast regions such as New York state (35.2%), the Washington D.C. area (15.5%), Illinois (15.2%), Boston/Massachusetts (13.8%), and New Jersey (10.4%). 

<br>

Okay, we finished learning about our variables. Let's move onto the results! [Part 2](/pages/grocery-part2)

<br>

