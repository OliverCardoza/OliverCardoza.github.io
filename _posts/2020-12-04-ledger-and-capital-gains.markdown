---
layout: post
title: "Ledger and Capital Gains"
date: 2020-12-04 16:39:00
---

tl;dr - I outline a strategy to manage capital gains with Ledger.

I recently wrote about capital gains in [Capital Gains 101](https://olivercardoza.com/2020/06/09/capital-gains-101.html) and since then I’ve been working out a strategy to properly track it in [Ledger](https://www.ledger-cli.org/) (for background see [The Path to Ledger](https://olivercardoza.com/2016/06/18/the-path-to-ledger.html)). This article outlines the process I settled on.

## Naive Model

When I originally got into Ledger I started by using the strategy outlined in the documentation: [Ledger: Total commodity prices](https://www.ledger-cli.org/3.0/doc/ledger3.html#Total-commodity-prices). It used the following transaction as an example which tracked a capital gain:

```
2012-04-10 My Broker
    Assets:Brokerage           10 AAPL @ $50.00
    Assets:Brokerage:Cash      $-500.00

2012-04-10 My Broker
    Assets:Brokerage:Cash      $375.00
    Assets:Brokerage           -5 AAPL {$50.00} @@ $375.00
    Income:Capital Gains       $-125.00
```

The first transaction purchases 10 AAPL stocks for $50 a piece. The second sells 5 for a total sale of $375. The second transaction includes the line `-5 AAPL {$50.00} @@ $375.00` which can be understood as:

* Sell 5 AAPL shares
* The shares individually cost $50 to acquire
* Receive total sale proceeds of $375 (or $75 per share)

The final line denotes the capital gain which is required in order for the transaction to balance to $0. At first glance this isn’t immediately obvious because it would appear that the $+375 cash proceeds are balanced by the $-375 sale proceeds. The unbalanced $125 comes from the `{$50.00}` acquisition price. When this is included, ledger also calculates the difference in value between acquisition and sale. In this case acquisition was `$50 * 5 = $250` and sale was implicitly `$75 * 5 = $375`. The difference is `$125` which is balanced by the `Income:Capital Gains` value of `$-125`.

## Flaws

Even though I have used this format over the last 5 years, I came to realize how flawed it is while writing my previous capital gains article. The primary drawback is that it does not account for any trade fees. This is a crucial variable in the adjusted cost basis (ACB) calculation. It is also not particularly helpful in helping you track your ACB across potentially dozens of transactions buying and selling a commodity.

## New Model

To outline my new model for calculating capital gains I’ll start by outlining some example transactions which buy and sell the VAB index fund:

```
2020-01-02 * Trade - BUY
    Assets:Brokerage             $-1100
    Expenses:Trading Fees        $100
    Assets:Brokerage             VAB 10 @ $100
 
2020-01-03 * Trade - BUY
    Assets:Brokerage             $-1600
    Expenses:Trading Fees        $100
    Assets:Brokerage             VAB 10 @ $150
 
2020-01-04 * Trade - SELL
    Assets:Brokerage             $1900
    Expenses:Trading Fees        $100
    Assets:Brokerage             VAB -10 @ $200
    ; Capital gain? TODO
```

These transactions outline 2 purchases of VAB followed by 1 sale of VAB. This sale will incur a capital gain which needs to be calculated. From my prior post the capital gain requires knowing the FMV, ACB, and sale fees. The sale itself outlines the FMV of $200 per share or $2000 for the total of 10 shares sold. It also includes the sale fee of $100 which is fictitiously high. The only thing missing is the ACB which can only be calculated using the information of all prior trades of this commodity.

ACB is modified on every transaction for a given commodity so it will require some bookkeeping on each transaction. My prior post outlined how this can be done in a table which tracks a running total of costs. We can recreate this using a [Ledger virtual posting](https://www.ledger-cli.org/3.0/doc/ledger3.html#Virtual-postings).

I’m going to jump to the solution here and then outline how I arrived at it in steps:

```
; example_acb.ledger
2020-01-02 * Trade - BUY
    Assets:Brokerage             $-1100
    Expenses:Trading Fees        $100
    Assets:Brokerage             VAB 10 @ $100
    (Virtual:ACB:VAB)            VAB_ACB 10 @@ $1100
 
2020-01-03 * Trade - BUY
    Assets:Brokerage             $-1600
    Expenses:Trading Fees        $100
    Assets:Brokerage             VAB 10 @ $150
    (Virtual:ACB:VAB)            VAB_ACB 10 @@ $1600
 
2020-01-04 * Trade - SELL
    Assets:Brokerage             $1900
    Expenses:Trading Fees        $100
    Assets:Brokerage             VAB -10 @ $200
    (Virtual:ACB:VAB)            VAB_ACB -10 @ $135
    (Virtual:Capital Gains)      $550
```
 
Ok, so what’s happening here? I’ll explain it in four parts:

1. Virtual account name: `(Virtual:ACB:VAB)`
2. Commodity code: `VAB_ACB`
3. ACB amounts: `VAB_ACB 10 @@ $1100`
4. Capital gain: `$550`

### 1. Virtual Account Name

`(Virtual:ACB:VAB)`

The most basic property is the parenthesis `()` which mark this as a virtual posting. Virtual postings are ignored when validating the transaction balancing to zero. Then the actual name is somewhat a matter of preference. I like to prefix my virtual account names with `Virtual` to organize all virtual accounts in the same root. I then include `ACB` to denote that all postings/accounts within it are related to ACB calculations. Finally `VAB` is included as the commodity in question because capital gains and ACB are commodity dependent. This means that your capital gains/losses for VAB won’t impact your other funds. The explicit inclusion of `VAB` as the account suffix helps keep the isolation between funds.

### 2. Commodity code

`VAB_ACB`

This decision is more of a quirk related to how ledger reports historical commodity prices. Given the following transaction:

```
; acb.ledger
2020-01-02 * Trade - BUY
    Assets:Brokerage             $-1100
    Expenses:Trading Fees        $100
    Assets:Brokerage             VAB 10 @ $100
    (Virtual:ACB:VAB)            VAB 10 @ $110
```
 
There are 2 distinct prices for VAB, the first is $100/share and the second is $110/share. If you were to query ledger for the market value of the VAB in your `Assets:Brokerage` account you may expect it to say $1000, given that the posting in `Assets:Brokerage` is quoted at 10 VAB @ $100 each. However Ledger seems to only track the most recent price so your book value in your `Assets:Brokerage` account is actually calculated to be $1100 (based on the later virtual posting). You can test this yourself by saving the above transaction as a file say `acb.ledger` and then running the following command:

```
ledger bal Assets:Brokerage -H -X CAD -f acb.ledger
```

It returns $0 or empty because the $-1100 cost is being perfectly balanced by the 10 VAB calculated at $110 each.

We actually want to track distinct prices for the asset book value (price/share) and the ACB value ((price+fee)/share). To get around this limitation I add the `_ACB` suffix to commodities in these virtual postings so Ledger keeps the prices distinct.

### 3. Actual Amounts

Now that the account name and commodity code are less puzzling we can focus on the amounts. In the case of commodity purchases, we record the total cost of the transaction including fees as the ACB using the `@@` operator. So the first transaction which costs $1100 ($100 fee + 10 VAB shares * $100 each) looks like this:


```
2020-01-02 * Trade - BUY
    Assets:Brokerage             $-1100
    Expenses:Trading Fees        $100
    Assets:Brokerage             VAB 10 @ $100
    (Virtual:ACB:VAB)            VAB_ACB 10 @@ $1100
```
 
This is fairly simple because the total cost of the transaction is already clearly defined as the cash value leaving your brokerage account so $1100 appears twice. Things get a little more tricky for sales so let’s focus on the last transaction in the example:
 
```
2020-01-04 * Trade - SELL
    Assets:Brokerage             $1900
    Expenses:Trading Fees        $100
    Assets:Brokerage             VAB -10 @ $200
    (Virtual:ACB:VAB)            VAB_ACB -10 @ $135
    (Virtual:Capital Gains)      $550
```
 
Since capital gains is discussed in the next section we’ll focus on the `(Virtual:ACB:VAB)` line here. This line is important for 2 reasons:
 
1. First it helps us calculate capital gains for this sale.
1. Second it is necessary to deduct ACB being used in capital gains for this sale so it does not interfere with future sales.
 
We want to deduct the ACB of the units being sold. To do this we must first know the total number of units held and the total ACB of these units. This can be accomplished with a few ledger queries:
 
```
# Total units held up to trade:
ledger bal Virtual:ACB:VAB --end 2020-01-04 -f example_acb.ledger
    VAB_ACB 20 Virtual:ACB:VAB
 
# Total ACB value of units held up to trade:
ledger bal Virtual:ACB:VAB --end 2020-01-04 -H -X $ -f example_acb.ledger
    $2700 Virtual:ACB:VAB
```
 
We can get the per-unit ACB by dividing the two values: `$2700 / 20 = $135`. This is the average cost to acquire the 20 VAB units from the first two trades. So in conclusion the line item is calculated to be the same number of units being sold with the unit price set to average ACB with calculation explained above.
 
### 4. Capital Gains
 
The final mystery left is how the capital gain is calculated in the sale to be `$550`. This is a basic plug-and-play calculation using the capital gains formula.
 
```
// 10 VAB units sold for $200 each
fmv = $2000
// $100 fee from the sale transaction
sale_fees = $100
// $135 is average ACB per calculation from before
acb = cost_to_acquire_units_sold
    = units_sold * average_unit_cost
    = 10 * $135
    = $1350
capital_gains = fmv - acb - sale_fees
              = $2000 - $1350 - $100
              = $550
```
 
And there you have it. Capital gains now being managed nicely in your ledger.
 
## Conclusion
 
While this method works I do still find it a little hacky. I wish the commodity naming `VAB_ACB` was unnecessary and that I could somehow leverage ledger’s double-entry validation to check my work. The latter is disabled by using virtual accounts.
 
My real judgement on this method is how it compares with how I was doing it before. Even though I started using ledger-cli in ~2015, I have been calculating capital gains separately using a spreadsheet when I’ve filed my taxes. I didn’t really like exporting my ledger data just to run a few calculations but it did the trick. Since I’m mostly purchasing stocks and not selling too often this hasn’t been too much work.
 
However, I’ve also started to get into cryptocurrencies which the [Canadian government recognizes as a commodity](https://www.canada.ca/en/revenue-agency/programs/about-canada-revenue-agency-cra/compliance/digital-currency/cryptocurrency-guide.html) for tax purposes. This means each transaction with crypto will require a capital gains calculation. At this point it made more sense to me to automate this work with some tooling.
 
As always I’m interested in any feedback on this post. Feel free to drop me a line at me@olivercardoza.com or [@OliverCardoza](https://twitter.com/OliverCardoza) on Twitter.

