---
layout: post
title: "Capital Gains 101"
date: 2020-06-09 21:57:00
---

tl;dr - This post explains the basics of capital gains in Canada using progressively more complicated examples.

## Why

As soon as you start investing you need to worry about calculating capital gains when filing your taxes. In my experience, it’s one of the most time-consuming steps because you can’t easily auto-import or blindly copy numbers from one form to another. I wrote this article to share what I’ve learned so far in the hopes it can help demystify the process for first-time investors.

## Tax Implications

In Canada we are required to report the capital gains and losses of investments at tax time. A capital gain occurs when you sell an investment at a higher value than it was acquired at. A capital loss is what happens when you sell at a lower price. Capital gains and losses don’t really exist from a tax perspective until you have sold an investment. So if you don’t sell anything then you won’t have to worry about this.

Another notable caveat is that capital gains are exempt from registered accounts like TFSAs and RRSPs. So these rules only apply to assets held outside of registered accounts.

The government taxes capital gains similar to income. The flipside is also true where a capital loss can be claimed for a tax deduction. One interesting difference between earning $100 in income vs a $100 capital gain is that only a percentage of the gain is taxed. At the time of this writing (2020) 50% of capital gains are added as taxable income. So if your marginal tax rate is 25% then for $100 income you’d be taxed $25. However, for $100 of capital gain only $50 is taxable so you’d only be taxed $12.50 (one quarter of $50).

<img src="https://docs.google.com/drawings/d/e/2PACX-1vTckmAOCQCclionBZDtKcnuj8NPHW7k69A8kSKRHHRQpCCjKnQWMkSnOuGq-O2u2jr33zWMXDw6tCAg/pub?w=518&amp;h=432" style="display: block; margin: auto;">

## 1. Simple Capital Gains

To help understand capital gains, I’ll use an example which purchases and sells a Vanguard index fund [VAB](https://www.vanguardcanada.ca/individual/indv/en/product.html#/fundDetail/etf/portId=9552/assetCode=bond/?overview). I myself have invested in this fund which tracks the Canadian aggregate bond market.

We’ll start by purchasing 10 VAB stocks for $100 each. This trade will cost us a $10 fee so in total it is $1010 of cash. Then some time later we’ll sell when the price hits $300 per VAB. This trade will also cost us a $10 fee so we’ll receive $2990 in cash (300 * 10 - 10). NOTE: The trading fee and the change in VAB price of this example are not realistic.

    # Purchase
    $1010 → 10 VAB @ $100/each + $10 fee

    # Sale
    10 VAB @ $300/each → $2990 + $10 fee

This is all of the information required to make a capital gain calculation. In simple terms the capital gain is the amount of money made from the above trade. We started by spending $1010 cash for the purchase and we ended up with $2990. By subtracting we get $1980 cash gain which is the capital gain.

The more rigorous definition and formula for capital gain is the **fair market value (FMV)** minus the **adjusted cost base (ACB)** and any other costs (aka. outlays and expenses on disposition). The below math shows how the above trade would be mapped to these terms and arrives at the same $1980 gain:

    # FMV = Fair Market Value = Value of sale 
    fmv = value of sale
        = 10 stocks * $300
        = $3000

    # ACB = Adjusted Cost Base = Cost of purchase
    acb = cost of purchase
        = 10 stocks * $100 + $10 fee
        = $1010


    # Capital Gains / Losses
    capital_gains = fmv - acb - sale_fees
    capital_gains = fmv - acb - sale_fees
                  = $3000 - $1010 - $10
                  = $1980

This simple example puts some substance behind ACB and FMV. The calculation here is roughly what you should expect to do when filing your taxes. Most tax software will ask you to enter the FMV, ACB, and sale fees, so it can do the basic arithmetic to calculate capital gains.

So far I’ve used loose definitions of *FMV = value of sale* and *ACB = cost of purchase*. In the next example I’ll touch on the *adjusted* part of *adjusted cost base*.

## 2. Averaging purchase cost for ACB

The next level of complexity might be if we make multiple purchases at different costs before making a sale. For example, let’s say we purchase 10 VAB for $100 each, and then another 10 VAB for $150 each. Finally we sell all 20 VAB for $200 each. Assume each transaction has a $10 fee.

The ACB is the cost of the purchases for units being sold. Since the units being sold had 2 different costs they now need to be averaged:

    average_cost = total_cost / num_units
                 = (purchase_cost1 + purchase_cost2)
                   / (purchased_units1 + purchased_units2)
                 = ($1010 + $1510) / (10 + 10)
                 = $2520 / 20
                 = $126

The ACB calculation can then be calculated:

    acb = cost of purchase
        = average_cost * units_sold
        = $126 * 20
        = $2520

And finally the full capital gains:

    fmv = value of sale
        = 20 stocks * $200
        = $4000

    capital_gains = fmv - acb - sale_fees
                  = $4000 - $2520 - $10
                  = $1470

## 3. Impact of Sales on ACB 

The previous example illustrates why the average cost is required to calculate ACB from multiple purchases. The final example will go through what happens if multiple purchases and sales are made. Let’s say we make the following sequence of trades:

* purchase 10 VAB for $200 each, $10 fee
* purchase 10 VAB for $300 each, $10 fee
* sell 10 VAB for $100 each, $10 fee
* purchase 10 VAB for 150 each, $10 fee
* sell 20 VAB for $100 each, $10 fee

Perhaps the simplest way to understand capital gains for this example is with a table:

{:class="table-bordered"}
|                                    | Units Owned | Cost of Purchase | Average Cost |  ACB  | Total Cost - Total ACB | Capital Gain / Loss |
|------------------------------------|:-----------:|:----------------:|:------------:|:-----:|:----------------------:|:-------------------:|
| Purchase 10 @ $200 price + $10 fee |          10 |            $2010 |         $201 | N/A   |                  $2010 | N/A                 |
| Purchase 10 @ $300 price + $10 fee |          20 |            $3010 |         $251 | N/A   |                  $5020 | N/A                 |
| Sell 10 @ $100 price + $10 fee     |          10 | N/A              |         $251 | $2510 |                  $2510 |              -$1520 |
| Purchase 10 @ $150 price + $10 fee |          20 |            $1510 |         $201 | N/A   |                  $4020 | N/A                 |
| Sale 20 @ $100 price + $10 fee     |           0 | N/A              |           $0 | $4020 |                     $0 |              -$2030 |

&nbsp;

The first three transactions are effectively the same as example 2. When calculating capital gains for the first sale the average cost per unit is $251 ($2010 first purchase, $3010 second purchase, averaged over 20 units). This results in the following capital gains calculation to arrive at -$1520 for the first sale (negative means capital loss!):

    fmv = value of sale
        = 10 stocks * $100
        = $1000

    acb = average_cost * units_sold
        = $251 * 10
        = $2510

    capital_gains = fmv - acb - sale_fees
                  = $1000 - $2510 - $10
                  = -$1520

Now the focus of this example is how we calculate the final capital gain (-$2030). This is tricky because now you are mixing units purchased across 3 prices with some units already sold. To calculate the ACB of the final transaction a more complex average is required. Previously we would have done the following **which is wrong!**

    # WARNING THIS IS WRONG
    average_cost = total_cost / num_units
                 = (purchase_cost1 + purchase_cost2 + purchase_cost3)
				   / (purchased_units1 + purchased_units2 + purchased_units3)
                 = ($2010 + $3010 + $1510) / (10 + 10 + 10)
                 = $217.67

The problem with this calculation is that it is the average cost of **ALL** units purchased rather than just those being sold. To do this properly we must only include units remaining by removing the ACBs of sold units:

    average_cost = (total_costs - total_acbs) / num_units
                 = ($2010 + $3010 + $1510 - $2510) / 20
                 = $201

You can see the running value of `total_costs - total_acbs` in the table above. That is how the average cost of $201 is found and the resulting capital gain:

    acb = average_cost * units_sold
        = $201 * 20
        = $4020

    fmv = 20 stock * $100
        = $2000

    capital_gains = fmv - acb - sale_fees
                  = $2000 - $4020 - $10
                  = -$2030

I personally found the subtraction of ACBs when performing the average calculation to be the most unintuitive part. One notion which helps is to think about how `average_cost` changes during a sale. If I purchased 20 units at an average unit cost of $251 and sold 10 the result is that my remaining 10 still have the same average unit cost of $251. You can see this represented in the table.

## Takeaways

Simple transaction history which involves purchasing X units of $FOO and then selling all X units are very straightforward when it comes to capital gains.

As you start to make multiple purchases and sales at varying prices the amount of calculations and state tracking starts to grow. This post also didn’t get into the complexities of what happens when you make purchases using USD on stocks priced in USD (hint: now add currency conversion to your list of problems).

The amount of complexity here normally justifies a spreadsheet of some kind. For past years I’ve kept a spreadsheet to track ACBs, FMVs, and all related fees. For next year I plan to come up with a less manual effort building on top of my [Ledger](https://ledger-cli.org) files and scripts. More on that in a followup post.

## Helpful Resources

The official documentation from the Canadian Revenue Agency can be found in this article: [CRA - Capital Gains](https://www.canada.ca/en/revenue-agency/services/forms-publications/publications/t4037/capital-gains.html). If you find that too intimidating (I did), then I recommend this article from Wealthsimple: [WealthSimple - Capital Gains Tax](https://www.wealthsimple.com/en-ca/learn/capital-gains-tax-canada).

This post has been edited, see post history: [GitHub](https://github.com/OliverCardoza/OliverCardoza.github.io/commits/master/_posts/2020-06-09-capital-gains-101.markdown).

