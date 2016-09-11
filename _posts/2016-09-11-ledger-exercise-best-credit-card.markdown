---
layout: post
title:  "Ledger Exercise: Best Credit Card"
date:   2016-06-18 14:53:00
---
Recently I was talking with some friends about credit cards; specifically which one offers the best rewards. At some point during the conversation I realized that my work with [Ledger](http://ledger-cli.org) had set me up to provide an interesting case study. It could also settle whether we were all being silly and debating over superficial gains. Skip to the end if you just want to see the fancy graph result.

So first I decided to pick my candidate cards to compare. I only chose cards which return cash or reward points that are pinned to a dollar value (e.g. 1 point == $0.01). I avoid other cards because without having reward points pinned the reward company can change the value whenever they want. For example if a flight from Toronto to Vancouver was worth 1000 Aeroplan points what's the dollar value per point? Well the answer is that the flight dollar and point value can change daily.

Ok back to candidate cards, I selected these three which I'm considering:

* [PC World Elite MasterCard](http://www.pcfinancial.ca/WorldElite) (current card)
    * Annual Fee: None
    * Effective reward value: 3% cash back at preferred merchants, 1% elsewhere
    * Preferred merchants: PC brand (Zehrs, Shoppers Drug Mart, Loblaws, etc)
    * Rewards redeemable: purchases at PC grocery stores
* [Tangerine Money-Back Credit Card](https://www.tangerine.ca/moneybackcreditcard)
    * Annual Fee: None
    * Effective reward value: 2% cash back in 2 categories, 1% elsewhere (+1 category if you use Tangerine savings account)
    * Potential Categories: groceries, furniture, restaurants, hotels, gas, recurring bills, drug store, home improvement, entertainment, public transpot and parking 
* [MBNA World Elite MasterCard](https://rewards.mbna.ca/worldelite)
    * Annual Fee: $85
    * Effective reward value: 2% cash back on all purchases

For each card I'll calculate the effective cashback percentage using my historical purchases over the last 20 months. This obviously isn't definitive since it uses my purchases but its potentially interesting nonetheless. I will calculate effective cashback percentage as follows:

```
effective_cashback = (cashback - fees) / total_spend
```
I'll present Ledger commands and run them on my transactions but won't post the actual values. Instead I'll present my `total_spend` as $1000 and present all other numbers relative to that.

### PC World Elite MasterCard

To identify the max rewards returned for this card I'll need to calculate two variables: `preferred_merchant_spend` and `other_spend`. The first tracks the amount I've spent at PC preferred merchants and the second tracks all other credit card expenses.

The preferred merchants which I used are: Esso, No Frills, Real Canadian Superstore, Shoppers Drug Mart, and Zehrs.

With the above information I can perform the following two Ledger reports to calculate rewards:

```
// Returns total credit card spending
ledger bal "Liabilities:Credit Cards:PC" -l "amount < 0"
     $1000

// Returns credit card spending at preferred merchants
ledger bal "Liabilities:Credit Cards:PC" -l "amount < 0 and payee =~ /Esso|(No Frills)|(Real Canadian Superstore)|(Shoppers Drug Mart)|Zehrs/"
     $99

// Now to calculate effective_cashback
total_spend = $1000
preferred_merchant_spend = $99
other_spend = total_spend - preferred_merchant_spend = $901
cashback = preferred_merchant_spend * 0.03 + other_spend * 0.01
         = $99 * 0.03 + $901 * 0.01
         = $2.97 + $9.01
         = $11.98
effective_cashback = (cashback - fees) / total_spend
                   = ($11.98 - $0) / $1000
                   = 1.20%
```

### Tangerine Money-Back Credit Card

I personally have a Tangerine savings account so this lets me pick three categories. Next is to figure out the best three. Thankfully I already have a head start on the report for this analysis on my [other Ledger post](http://olivercardoza.com/2016/06/18/the-path-to-ledger.html). There is however, one modification I need to make. We want to limit expense transactions to those that occurred from my credit card. Thankfully Ledger has us covered.

```
// Returns top expenses from credit card. Remember normalized to $1000 total credit purchases.
ledger bal --flat -S -T Expenses and expr 'any(account =~ /Liabilities:Credit Cards:PC/)'
    $137 Expenses:Food:Restaurants
    $112 Expenses:Shopping:Electronics
     $87 Expenses:Food:Groceries
     $67 Expenses:Car:Gas
     $55 Expenses:Vacation:Flights 
         ...
```

Now I can calculate effective cashback using my top-spending, supported categories: restaurants, groceries, and gas.
```
total_spend = $1000
preferred_category_spend = $137 + $87 + $67
                         = $291
other_spend = $709
cashback = preferred_category_spend * 0.02 + other_spend * 0.01
         = $291 * 0.02 + $709 * 0.01
         = $5.82 + $7.09
         = $12.91
effective_cashback = (cashback - fees) / total_spend
                   = ($12.91 - $0) / $1000
                   = 1.29%
```

### MBNA World Elite MasterCard

This card should be the easiest one to calculate the effective cashback rate because it does not disciminate purchases using preferred merchants and categories. However, since we are using a normalized spending of $1000 the $85 annual fee will eat up all the cashback. I'll show two examples to illustrate this point.
```
total_spend = $1000
cashback = total_spend * 0.02
         = $1000 * 0.02
         = $20
effective_cashback = ($20 - $85) / $1000
                   = -6.5%

total_spend = $20000
cashback = total_spend * 0.02
         = $20000 * 0.02
         = $400
effective_cashback = ($400 - $85) / $1000
                   = 1.58%
```

This means that there is a threshold of spending where if you spend more than the threshold value the MBNA card will have the best cashback return.

## Summary

The Tangerine and the PC credit card provide almost the same return rate for my purchases but the Tangerine card comes out on top. The MBNA card provides the best return after annual credit spending reaches $12000.

<iframe width="613" height="379" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1BWTzVrlXIEFNEa-3XXyqXpADdyev8BOcfUSuHkMQkpw/pubchart?oid=789365586&amp;format=interactive"></iframe>

Was this a worthwhile analysis? Well I learned more about using Ledger [value expressions](http://ledger-cli.org/3.0/doc/ledger3.html#Value-Expressions) and limit param. In the big picture my choice of credit card won't have a big impact on my life. But sometimes running through the analysis can be fun :)
