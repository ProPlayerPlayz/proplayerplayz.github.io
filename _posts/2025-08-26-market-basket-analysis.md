---
title: "Market Basket Analysis"
author: "Sakthi Swaroopan S"
format: html
editor: visual
---

# Market Basket Analysis

## 1. Introduction

Market Basket Analysis is a method to understand the purchasing behavior/choice of customers. Based on the frequency of purchases(support) and associations of items(confidence) we can develop rules to predict the items in the customer basket.

------------------------------------------------------------------------

## 2. Market Basket Analysis using Groceries Dataset from Kaggle

### 2.1. Preliminary Data Exploration


```
##   Member_number       Date  itemDescription
## 1          1808 21-07-2015   tropical fruit
## 2          2552 05-01-2015       whole milk
## 3          2300 19-09-2015        pip fruit
## 4          1187 12-12-2015 other vegetables
## 5          3037 01-02-2015       whole milk
## 6          4941 14-02-2015       rolls/buns
```

```
##       Member_number       Date       itemDescription
## 38760          3364 06-05-2014                   oil
## 38761          4471 08-10-2014         sliced cheese
## 38762          2022 23-02-2014                 candy
## 38763          1097 16-04-2014              cake bar
## 38764          1510 03-12-2014 fruit/vegetable juice
## 38765          1521 26-12-2014              cat food
```

```
## 'data.frame':	38765 obs. of  3 variables:
##  $ Member_number  : int  1808 2552 2300 1187 3037 4941 4501 3803 2762 4119 ...
##  $ Date           : chr  "21-07-2015" "05-01-2015" "19-09-2015" "12-12-2015" ...
##  $ itemDescription: chr  "tropical fruit" "whole milk" "pip fruit" "other vegetables" ...
```

```
## [1] 3898
```

The Groceries_Data from Kaggle has 38765 Observations and 3 Variables including Member_number, Date_of_Purchase and the items in the basket. The transaction data spans over the years 2014 and 2015.

------------------------------------------------------------------------

### 2.2. Preparing the data for Market Basket Analysis

The data is currently in a "row per item" format, we will need to convert this into "row per transaction" format to effectively perform the market basket analysis.



Using association rules package we get the "baskets" of items from the data to use with the apriori algorithm to find association rules



We use these transactions to get a result of combinations of the items in the "basket" along with values such as support, confidence and list which help us determine the likelihood that the customer buys a certain item given they have already picked out certain items. We will display the top 10 items in this list to get an idea of how our result looks like


```
## Apriori
## 
## Parameter specification:
##  confidence minval smax arem  aval originalSupport maxtime support minlen maxlen target  ext
##         0.2    0.1    1 none FALSE            TRUE       5   5e-04      2     10  rules TRUE
## 
## Algorithmic control:
##  filter tree heap memopt load sort verbose
##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
## 
## Absolute minimum support count: 7 
## 
## set item appearances ...[0 item(s)] done [0.00s].
## set transactions ...[167 item(s), 14963 transaction(s)] done [0.00s].
## sorting and recoding items ... [158 item(s)] done [0.00s].
## creating transaction tree ... done [0.00s].
## checking subsets of size 1 2 3 4 done [0.00s].
## writing ... [19 rule(s)] done [0.00s].
## creating S4 object  ... done [0.00s].
```

```
##      lhs                                rhs          support      confidence coverage    lift     count
## [1]  {artif. sweetener}              => {whole milk} 0.0005346521 0.2758621  0.001938114 1.746815  8   
## [2]  {brandy}                        => {whole milk} 0.0008688097 0.3421053  0.002539598 2.166281 13   
## [3]  {spices}                        => {soda}       0.0006014837 0.2250000  0.002673261 2.317051  9   
## [4]  {softener}                      => {whole milk} 0.0008019782 0.2926829  0.002740092 1.853328 12   
## [5]  {house keeping products}        => {whole milk} 0.0007351467 0.2444444  0.003007418 1.547872 11   
## [6]  {finished products}             => {whole milk} 0.0008688097 0.2031250  0.004277217 1.286229 13   
## [7]  {rolls/buns, white bread}       => {whole milk} 0.0006014837 0.2812500  0.002138609 1.780933  9   
## [8]  {other vegetables, white bread} => {whole milk} 0.0005346521 0.2051282  0.002606429 1.298914  8   
## [9]  {margarine, soda}               => {whole milk} 0.0005346521 0.2051282  0.002606429 1.298914  8   
## [10] {curd, rolls/buns}              => {whole milk} 0.0006014837 0.2195122  0.002740092 1.389996  9
```

We can sort the output based on confidence for a clearer picture.


```
##      lhs                          rhs                support      confidence coverage    lift     count
## [1]  {pork, sausage}           => {whole milk}       0.0006014837 0.3913043  0.001537125 2.477819  9   
## [2]  {brandy}                  => {whole milk}       0.0008688097 0.3421053  0.002539598 2.166281 13   
## [3]  {softener}                => {whole milk}       0.0008019782 0.2926829  0.002740092 1.853328 12   
## [4]  {rolls/buns, white bread} => {whole milk}       0.0006014837 0.2812500  0.002138609 1.780933  9   
## [5]  {artif. sweetener}        => {whole milk}       0.0005346521 0.2758621  0.001938114 1.746815  8   
## [6]  {sausage, shopping bags}  => {other vegetables} 0.0005346521 0.2758621  0.001938114 2.259291  8   
## [7]  {sausage, yogurt}         => {whole milk}       0.0014702934 0.2558140  0.005747511 1.619866 22   
## [8]  {house keeping products}  => {whole milk}       0.0007351467 0.2444444  0.003007418 1.547872 11   
## [9]  {pastry, soda}            => {whole milk}       0.0009356412 0.2295082  0.004076723 1.453293 14   
## [10] {pastry, sausage}         => {whole milk}       0.0007351467 0.2291667  0.003207913 1.451130 11
```

Taking a look at the top 10 rows in the confidence sorted results we can observe that "whole milk" has a high likelihood of being picked when "pork" and "sausage" are also already picked. We also observe similar relation ships between the "LHS" and the "RHS" column. The Results give us an idea of the probability that the "RHS" item is taken when we already have "LHS" items.
