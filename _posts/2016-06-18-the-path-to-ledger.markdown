---
layout: post
title:  "The Path to Ledger"
date:   2016-06-18 14:53:00
---
When I started working full-time my financial life got a lot more complicated. I created a registered retirement savings plan account (RRSP) because my employer matched my contributions. I needed to create a brokerage account because my employer partially paid me in equity (company stock). An additional complication is that the stocks are sold for US dollars and I'm based on Canada. So I opened a USD account to hold the revenue from selling stock. I ended up having 9 financial open accounts.

I started off trying [Mint](https://www.mint.com/) to help make sense of all the data. It's a website which you give all of your financial account passwords and in return it provides a single location to dig into all of your data. Budgets, dashboards, graphs, and more which appealed to my inner spreadsheet fiend. Massive security and privacy concerns aside, it does a decent job for a few accounts (chequing account, credit card). However, it had flaky support for some of my accounts and no support for others. Eventually the lack of support and the sharing passwords got to me and I decided to look into a better alternative.

This is how I found [Ledger](http://ledger-cli.org/): a data format and associated command line tool to organize financial data. It is a [double-entry accounting system](https://en.wikipedia.org/wiki/Double-entry_bookkeeping_system) which means that every transaction lists a debtor account (where the money comes from) and a credit account (where the money goes). All transactions are stored in ledger format in text files and the `ledger` command is used to read them. Here's a simplified example transaction from my ledger:

```
2015/08/30 Starbucks
    Expenses:Food                     $3.50
    Liabilities:MasterCard           $-3.50
```

This transaction shows that I paid $3.50 at Starbucks using my MasterCard. The transaction total balances to 0 which is a principle of double-entry accounting. Money is not created in a vaccuum. It comes from somewhere and goes somewhere else. Unless of course you're a [central bank](https://en.wikipedia.org/wiki/Central_bank) like the Bank of Canada. Then your ledger would look like:

```
2016/01/01 Canadian Mint
    Assets:Coins                     $1,000,000
    Expenses:Materials               $100,000
    ???                              $-900,000
```

Neglecting the `???` account would cause this transaction to be invalid and not balance. This leads me to believe that central banks shouldn't use double-entry accounting ☺

The Ledger format is incredibly flexibile and the reporting tools very powerful. Here's an example of a really complicated transaction involving a stock sale (amounts edited):

```
2016-06-18 * Stock sale
    Assets:Brokerage                AAPL -2.00 {CAD 100.00} @ CAD 130.00
    Assets:Chequing:USD             USD 199 @ CAD 1.30
    Expenses:Trading Fees           CAD 1.30
    Income:Capital Gains            CAD -60.00
```

Here 2 stocks of AAPL (Apple Inc.) were sold for 100 USD each with a conversion rate of 1.30 CAD/USD. The stocks were previously purchased at a price of 100 CAD and is now being sold for a value of 130 CAD which incurs a capital gain of 60 CAD. Additionally a trading fee was chared which is deducted from the amount paid to the USD chequing account.

Another neat reporting feature of Ledger is being able to understand where you are spending your money proportionally. After some iteration I was able to craft the following Ledger report command:

```
ledger bal Expenses -H -X CAD --flat -S -T
```

Describing the arguments from left to right:

* `ledger`: the name of the command line tool
* `bal`: run a balance report [[docs]](http://ledger-cli.org/3.0/doc/ledger3.html#Balance-Reports)
* `Expenses`: limit the scope of accounts to those nested under Expenses
* `-H -X CAD`: value commodities at the time of their acquisition (-H) and convert all values to CAD (-X CAD) [[docs]](http://ledger-cli.org/3.0/doc/ledger3.html#index-_002d_002dlot_002dprices-3)
* `--flat`: use the full name of accounts rather than indented tree [[docs]](http://ledger-cli.org/3.0/doc/ledger3.html#index-_002d_002dflat)
* `-S -T`: sort the accounts (-S) in descending order of account total (-T)

I wrote this command for curiousity to see where my top spending categories have been for the last year and a half of Ledger reporting. Here's the output with cash totals mocked:

```
$ ledger bal Expenses -H -X CAD --flat -S -T 
    CAD 900  Expenses:Bills and Utilities:Rent
    CAD 800  Expenses:Vacation
    CAD 700  Expenses:Food:Restaurants
    CAD 600  Expenses:Shopping:Electronics
    CAD 500  Expenses:Food:Groceries
    CAD 400  Expenses:Car:Gas
    CAD 300  Expenses:Shopping:Sporting Goods
    CAD 200  Expenses:Health and Fitness:Gym
    CAD 100  Expenses:Cash
    CAD   1  Expenses:Shopping:Gifts
```

If you want to learn more about Ledger check out the following resources:

* [Ledger HTML docs](http://ledger-cli.org/3.0/doc/ledger3.html#Introduction-to-Ledger)
* [Wiki: double-entry bookkeeping system](https://en.wikipedia.org/wiki/Double-entry_bookkeeping_system)
 
I generally don't recommend Ledger because normal people don't want to boot up a terminal to do their accounting. Also most people (hopefully) have a simpler financial situation. But if you're weird and complicated like me and you don't mind writing import scripts and using Linux tools then this might be the perfect fit.

Also if you have feedback or requests about this post email me@olivercardoza.com
