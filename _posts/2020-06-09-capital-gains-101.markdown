---
layout: post
title: "Capital Gains 101"
date: 2020-06-09 21:57:00
---

tl;dr - This post explains the basics of capital gains in Canada using progressively more complicated examples.

## Tax Implications

In Canada we are required to report the capital gains and losses of investments at tax time. A capital gain is only realized when you sell an investment at a higher value than it was acquired at. A capital loss is what happens when you sell at a lower price. Capital gains and losses don’t really exist from a tax perspective until you have sold an investment.

The government taxes capital gains similar to income. A percentage of the capital gain is taxed as if it were supplemental income. At the time of this writing (2020) 50% of capital gains are added as taxable income.

The official documentation from the Canadian Revenue Agency can be found in this article: [CRA - Capital Gains](https://www.canada.ca/en/revenue-agency/services/forms-publications/publications/t4037/capital-gains.html). If you find that too intimidating (I did), then I recommend this article from Wealthsimple: [WealthSimple - Capital Gains Tax](https://www.wealthsimple.com/en-ca/learn/capital-gains-tax-canada). This post will attempt to explain capital gains calculations through increasingly complicated examples.

## 1. Simple Capital Gains

The basic formula for capital gains is the following:

    # FMV = Fair Market Value = Value of sale
    # ACB = Adjusted Cost Base = Cost of purchase
    capital_gains = fmv - acb - sale_fees

There is a little to unpack with these terms like ACB and FMV. To help explain, we can use a simple example which purchases and sells the Vanguard index fund [VAB](https://www.vanguardcanada.ca/individual/indv/en/product.html#/fundDetail/etf/portId=9552/assetCode=bond/?overview) which tracks the Canadian aggregate bond market. We’ll start by purchasing 10 VAB for CAD 100 each and then selling at a later time for CAD 300 each. Let’s also say that both the purchase and sale had a CAD 10 fee each.
 
    fmv = value of sale
        = 10 stocks * CAD 300
        = CAD 3000
    acb = cost of purchase
        = 10 stocks * CAD 100 + CAD 10 fee
        = CAD 1010
    capital_gains = fmv - acb - sale_fees
                  = CAD 3000 - CAD 1010 - CAD 10
                  = CAD 1980

This simple example puts some substance behind ACB and FMV. The calculations here are also roughly what is required when you are filing your taxes. Most tax software will ask you to enter the FMV, ACB, and sale fees, so it can do the basic arithmetic to calculate capital gains. This calculation isn’t too bad, the complication often comes from more advanced ACB calculations. I haven’t really touched on the *adjusted* part of *adjusted cost base*.

## 2. Averaging purchase cost for ACB

The next level of complexity might be if you make multiple purchases at different costs before making a sale. For example, let’s say you purchase 10 VAB for CAD 100 each, and then another 10 VAB for CAD 150 each. Finally you sell all 20 VAB for CAD 200 each. Assume each transaction has a CAD 10 fee.

The ACB now requires finding the average unit cost of VAB. In the earlier example there was only 1 purchase so it was the average. With 2 purchases the ACB now requires an average:

    average_cost = total_cost / num_units
                 = (purchase_value1 + fee1 + purchase_value2 + fee2)
                   / (purchased_units1 + purchased_units2)
                 = (10 * CAD 100 + CAD 10 + 10 * CAD 150 + CAD 10)
                   / (10 + 10)
                 = CAD 2520 / 20
                 = CAD 126

The ACB calculation can then be calculated:

    acb = average_cost * units_sold
        = CAD 126 * 20
        = CAD 2520

And finally the full capital gains:

    fmv = value of sale
        = 20 stocks * CAD 200
        = CAD 4000
    acb = CAD 2520
    capital_gains = fmv - acb - sale_fees
                  = CAD 4000 - CAD 2520 - CAD 10
                  = CAD 1470

## 3. Impact of Sales on ACB 

The previous example illustrates why the average cost is required to calculate ACB from multiple purchases. The final example will go through what happens if multiple purchases and sales are made. The key detail is that `average_cost` does not change when investments are sold. Let’s say you make the following sequence of trades:

* purchase 10 VAB for CAD 200 each, CAD 10 fee
* purchase 10 VAB for CAD 300 each, CAD 10 fee
* sell 10 VAB for CAD 100 each, CAD 10 fee
* purchase 10 VAB for 150 each, CAD 10 fee
* sell 20 VAB for CAD 100 each, CAD 10 fee

For this example we will calculate capital gains for each sale. For the first sale it is similar to the last example:

    average_cost = total_cost / num_units
                 = (cost1 + cost2) / num_units
                 = (10 * CAD 200 + 10 + 10 * CAD 300 + 10) / 20
                 = CAD 5020 / 20
                 = CAD 251
    fmv = value of sale
        = 10 stocks * CAD 100
        = CAD 1000
    acb = average_cost * units_sold
        = CAD 251 * 10
        = CAD 2510
    capital_gains = fmv - acb - sale_fees
                  = CAD 1000 - CAD 2510 - CAD 10
                  = CAD -1520

In this case the sale had a capital loss of CAD 1520 denoted by the negative value. For the second sale the ACB calculation must take into account the remaining cost after the previous sale. A more complete ACB formula would be:

    acb = average_cost_remaining * units_sold

Where the average cost remaining (ACR) can be unpacked:

    acr = cost_remaining / units_remaining
    cost_remaining = sum(purchase_costs) - sum(sale_acbs)

Let’s calculate ACB now for the last transaction taking into account removing cost from the 3rd transaction sale and adding more from the 4th transaction purchase.

    cost_remaining = sum(purchase_costs) - sum(sale_acbs)
                   = purchase_cost1 + purchase_cost2 + purchase_cost4 - sale_acb3
                   = CAD 2010 + CAD 3010 + CAD 1510 - CAD 2510
                   = CAD 4020
    units_remaining = 20
    acr = cost_remaining / units_remaining
        = CAD 4020 / 20
        = CAD 201
    acb = acr * units_sold
        = CAD 201 * 20
        = CAD 4020

Finally capital gains:

    fmv = 20 stock * CAD 100
        = CAD 2000
    acb = CAD 4020
    capital_gains = fmv - acb - sale_fees
                  = CAD 2000 - CAD 4020 - CAD 10
                  = CAD -2030

Another big loss, ouch!

## Takeaways

Simple transaction history which involves purchasing X units of $FOO and then selling all X units are very straightforward when it comes to capital gains.

As you start to make multiple purchases and sales at varying prices the amount of calculations and state tracking starts to grow. This post also didn’t get into the complexities of what happens when you make purchases using USD on stocks priced in USD (hint: now add currency conversion to your list of problems).

The amount of complexity here normally justifies a spreadsheet of some kind. For past years I’ve kept a spreadsheet to track ACBs, FMVs, and all related fees. For next year I plan to come up with a less manual effort building on top of my [Ledger](https://ledger-cli.org) files and scripts. More on that in a followup post.

