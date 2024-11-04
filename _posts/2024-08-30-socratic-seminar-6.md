---
layout: post
type: socratic
title: "Socratic Seminar 6"
---


We will start with introductions, some basic ground rules, and jump into technical discussions. We will cover aspects of the bitcoin protocol, new research developments, recent news, and software developments.
# Announcements
-   No pictures or recordings
-   Thank you [OGBC](https://www.ogbc.com/) and [Lnfi Network](https://www.lnfi.network) for sponsorship of the event space and pizzas
-   Introductions
# Agenda
- [P2P Economy with Lightning & Fiber Network](https://docs.google.com/presentation/d/1LY14oP6CodN2LKt_kIjS6t2KdcB-Gi2gdD3wTYk8ZwY/edit) - Cipher, CELL Studio
- Bitcoin Basics #3 - Heng Hong
- Industry Updates, [Bitcoin Projects' Fundraising Updates](https://docs.google.com/spreadsheets/d/1k0I348eU-dEVKTTBVwfyyXozLjDCTqCPszLFGgJ7KOg/edit?usp=sharing) - Darius

# Industry Updates - Bitcoin News & Releases

#### [BIP Draft: "FROST Signing"](https://groups.google.com/g/bitcoindev/c/PeMp2HQl-H4)

The FROST (Flexible Round-Optimal Schnorr Threshold) signature scheme is designed to address key challenges in cryptographic signing processes by allowing a group of participants to jointly create a Schnorr signature using a threshold scheme. Areas of application: multi-sig wallets, DeFi whereby FROST are integrated into smart contracts to manage access controls, etc.

A BIP draft for the FROST threshold signing protocol has been proposed and is seeking feedback from the community.

#### [Bitcoin Core v27.1: Bug Fixes & Performance Improvements](https://bitcoincore.org/bin/bitcoin-core-27.1/ )

Another release of Core.

This release includes various bug fixes and performance improvements, as well as updated translations. Please report bugs using the issue tracker at GitHub.

[BOLT 12 Trusted Contacts](https://delvingbitcoin.org/t/bolt-12-trusted-contacts/1046)

Recap: BOLT 12 enhances the Lightning Network by introducing reusable offers, improving privacy through onion messages, and enabling flexible, feature-rich payment mechanisms. It supports static offers for multiple payments, simplifies recurring payments like subscriptions, and allows for automated refunds and reversals. With invoice requests, users gain more control and privacy over transactions, while standardization ensures interoperability across the network, making BOLT 12 a key step towards a more versatile and user-friendly Lightning Network.

The proposal outlines a method to enhance the user experience on the Lightning Network by enabling trusted contacts through BOLT 12, allowing users to manage a contacts list and make payments without needing to share a new invoice each time. The approach involves generating a `contact_key` for mutual authentication between users, with options for distributing this key via BOLT 12 offers or BIP21 URIs. Three options for contact mutual authentication are discussed: directly using `invreq_payer_id`, deriving a per-contact `invreq_payer_id`, or using the bLIP-31 protocol, with the second option being favored for balancing simplicity and privacy. The goal is to standardize this feature across different wallets to create a seamless payment experience.

#### [AntPool & Bitmain Acting as 'a Pool of Pools' - Report](https://www.nobsbitcoin.com/bitmain-antpool-pool-of-pools-report/)

"TLDR: AntPool/Bitmain is essentially a pool of pools. This is causing mining centralization,"

#### [Stable Channels](https://stablechannels.com/)

With a stable channel client, two users can rapidly settle a contract for difference so that one counterparty maintains a USD-equivalent bitcoin value so long as the other cooperates.

#### [Mutiny Wallet shutting down](https://blog.mutinywallet.com/mutiny-wallet-is-shutting-down/)

“TLDR: Our company is exploring alternative products, we’ll be shutting down the wallet at the end of the year but you can still self host.”

[NIP update - nostr marketplace (NIP-15)](https://github.com/nostr-protocol/nips/blob/master/15.md)

There's an LNBits extension that allows you to list and shop for goods using Nostr relays and get paid through Bitcoin/Lightning

[A Bitaxe Has Mined Block 853742](https://www.nobsbitcoin.com/a-bitaxe-has-found-a-block/)

A solo Bitcoin miner with 3 TH/s has mined block 853742, overcoming odds of 1 in 1.2 million and earning 3.192 BTC (approximately $200,000).

A Bitaxe costs ~US$440.