---
layout: post
type: socratic
title: "Lightning Network (from Bitcoin University)"
---

Lightning Network is a scaling solution that attempts to address Bitcoin's slow transaction speeds and high transaction costs.

LN: very fast, very low fees

The native money of the LN is BTC or SATS (1 BTC = 100M SATS)

LN is not controlled by any private company nor its development company - Lightning Labs

LN is an open, permissionless protocol that allows anyone to build on it or send or receive money using it

<u>At the start</u>
Christina 100,000 sats =========== Bob 50,000 sats

<u>Christina sends Bob 10,000 sats</u>
Christina 90,000 sats =========== Bob 60,000 sats

<u>Christina receives from Bob 20,000 sats</u>
Christina 110,000 sats =========== Bob 40,000 sats

To open up a Lightning channel, you and someone else make a 2/2 multi-sig transaction (using the Bitcoin network) that locks up BTC on-chain and then funds that channel with that BTC.

Whatever BTC you add, ends up on your side of the payment channel. Whatever BTC the other person adds, ends up on their side of the payment channel.

No additional BTC is created in the process, so there is no additional monetary inflation.

You can then send money back and forth in that Lightning channel

The LN is made up of 1000s of these two party payment channels.

Christina ===== Bob
Bob ===== John
Christina ===== Bob ===== John

At some point, Christina and Bob may choose to "settle up" by closing their channel by doing a transaction on the Bitcoin base layer. At that point, each person will get the sats that were on their side of the channel

Either person can close the channel whenever they want and claim the sats that rightly belong to them

[![JcNFgKN.md.png](https://iili.io/JcNFgKN.md.png)](https://freeimage.host/i/JcNFgKN)
[![JcNF4St.md.png](https://iili.io/JcNF4St.md.png)](https://freeimage.host/i/JcNF4St)
[![JcNFrlI.md.png](https://iili.io/JcNFrlI.md.png)](https://freeimage.host/i/JcNFrlI)
[![JcNFvov.md.png](https://iili.io/JcNFvov.md.png)](https://freeimage.host/i/JcNFvov)
[![JcNFeDJ.md.png](https://iili.io/JcNFeDJ.md.png)](https://freeimage.host/i/JcNFeDJ)
[![JcNFONa.md.png](https://iili.io/JcNFONa.md.png)](https://freeimage.host/i/JcNFONa)
[![JcNF8VR.md.png](https://iili.io/JcNF8VR.md.png)](https://freeimage.host/i/JcNF8VR)

[Lightning Network Map](https://mempool.space/lightning)

LN transactions are NOT broadcast to the entire network, unlike the Bitcoin network.

As a result, no one really knows how many transactions were actually taking place on the LN.

The only channels mapped here are channels that have publicly announced themselves and that are connected to nodes that are connected to mempool.space's Lightning node.

So there may be many payment channels that we don't know of.

Payment volume may also be much higher than we think.

Node A and Node B may also be much higher than we think.

Node A and Node B could be sending millions of dollars of Bitcoin back and forth to each other every month and we would never know

Also, Bitcoin's Taproot upgrade ensures that Lightning channel openings and closings look like regular simple spends on-chain.

This contributes to greater privacy.

# Problem with Lightning channels

There is a lot of rebalancing involved with running a channel.

Let's say that I have a Lightning channel open with my local grocery store (an illustrative example):
I'm usually the one spending the money and sending it to the groccery store when I buy food and I rarely need refunds, so most of the money is flowing one-way. And, most of the money in the channel ends up on their side.

If I want to add more money on my side of the channel, I could try to send more sats to myself if those sats are already on the LN.

But if they are not already on LN -- The grocery store and I will need to agree to close our Lightning channel, wait for me to add more sats from on-chain, and then reopen the channel.

That requires two on-chain transactions (closing and then reopening the channel -- which can get expensive when Bitcoin transaction fees are high)

And it also means that our channel is down for an hour (6 confirmations) or likely more.

Here's another problem that needs to be solved:

Unified wallets (Muun, Phoenix, Alby) which allow you transact both on-chain and on LN, while showing you a single total Bitcoin balance

How are these unified wallets supposed to swap back and forth between on-chain BTC and BTC that's held on the LN.

Alby and Muun solves this problem by holding your BTC onchain then doing a submarine swap when you need to send via LN.

Muun's model breaks down in a high transaction fee environment as we have seen in the past 12 months.

Phoenix utilises splicing, a new technology.

Splicing = the ability to easily add or remove sats from a Lightning channel without having to close and then reopen the channel.

Closing and reopening the channel would require 2 on-chain transaction fees, which can be problematic in a high fee environment.

"Splice in" = add funds to a Lightning channel
"Splice out" = remove funds from a Lightning channel

Splicing makes it much easier to design Bitcoin wallets that can easily send and receive and hold Bitcoin both on-chain and via the LN.

Splicing makes Bitcoin on-chain and Bitcoin on LN more fungible. 

That is a huge innovation. For example, you can make an on-chain payment using BTC that is held on the LN and vice versa.

We will do deep dive into the tech of Splicing in the future.