---
layout: post
title: "rate and return"
date: 2020-11-16 22:22:00
categories: INVESTMENTS
permalink: /archivers/rateandreturn
nocomments: false
use_math: true
---

# Rate and Return Difference

It is important to understand the difference between rate and return here.

A rate tells you how much you will get for 1 unit.

This is a common method of interaction when you go to any exchange on any street in any country.

For example, you go to an exchange in London and ask how much is the dollar rate, and they will tell you 1 pound = 2 dollars.

In this real-world example, the terms rate and return are equivalent, because the rate is linear, which means that for 2 pounds you'll get 4 dollars, for 3 pounds you'll get 6 dollars and so on.

On UniSwap's trading system (as in many other trading systems on the blockchain), rate and return are not equivalent.

For example, if your ETH/TKN spot-price is 10 on UniSwap, then it means that for 1 wei of your TKN, you will get 10 wei of ETH.

But for 1234 wei of your TKN, you will necessarily get less than 12340 wei of ETH.

This is because your conversion is subjected to slippage (loss).

It may lead you to think that this spot-price is a charade (a hoax).

But it is nevertheless useful for some measurements of a pool.

However, you should definitely not rely on this rate in order to calculate the expected return for some given amount.

In order to do that, you may use

> Y \* x / (X + x)

where:

> x is your input amount of source tokens

> X is the balance of the pool in the source token

> Y is the balance of the pool in the target token

Note that as your input amount gets closer to 1, the expected return becomes closer to the rate (i.e., the spot-price, which as quoted from your question at the top of this answer, is Y / X).

from https://ethereum.stackexchange.com/questions/83701/how-to-infer-token-price-from-ethereum-blockchain-uniswap-data/83702#83702?newreg=65b90e7bf2a344a6a4a6c6e152635daa
