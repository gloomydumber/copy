---
layout: post
title: "Uniswap How to Manually Approve ERC20 Tokens"
date: 2021-01-23 20:20:00
categories: INVESTMENTS
permalink: /archivers/uniApprove
nocomments: false
use_math: true
---

# Uniswap Manual Approve

## Swap Unavailable with Zero Balance on Uniswap

![unavailable](/assets/posts/2021-01-23-uniApprove/approveNoAvailable.png)

Uniswap UI won't let us do swap with Zero Balance ERC20 to ETH like Above Image.

## Silly way

![approvebutton](/assets/posts/2021-01-23-uniApprove/approveActive.png)

Just Deposit Some Extra ERC20 tokens and Click Approve MVL button like above Image?

This way is so bothersome and annoying.

## How to execute "Approve" function without Uniswap ?

![etherscan](/assets/posts/2021-01-23-uniApprove/ehterscanApprove.png)

Use EtherScan and excecute Approve() Function Directly.

First, visit below URL (Token Address! Don't type Pair Address or other Wrong Addresses)

> https://etherscan.io/token/{tokenAddress}#writeContractCheck 

Second, Connect your Wallet(e.g. Metamask) on Web3

Third, type below on spender (Uniswap Router V2 Address)

> 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D

Fourth, type infinite value in uint256

> 115792089237316195423570985008687907853269984665640564039457584007913129639935

## Result

![done](/assets/posts/2021-01-23-uniApprove/approveDone.png)

Approve() function executed by EtherScan Web3 API

based on [? Ethereum StackExchange Question](https://ethereum.stackexchange.com/questions/88064/how-to-manually-approve-a-token-for-swap-on-uniswap-direct-contract-interaction)

why contract of erc20 tokens have approve() function : [? reddit post](https://www.reddit.com/r/UniSwap/comments/hxb74e/why_does_uniswap_require_me_to_approve_a_token/)