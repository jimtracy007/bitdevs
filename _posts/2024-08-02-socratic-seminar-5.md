---
layout: post
type: socratic
title: "Socratic Seminar 5"
---


We will start with introductions, some basic ground rules, and jump into technical discussions. We will cover aspects of the bitcoin protocol, new research developments, recent news, and software developments.
# Announcements
-   No pictures or recordings
-   Thank you [OGBC](https://www.ogbc.com/) and [Lnfi Network](https://www.lnfi.network) for sponsorship of the event space and pizzas
-   Introductions
# Agenda
- DLC - Lucas, 10101
- [Bitcoin Basics #2 - Bitcoin Transactions](https://docs.google.com/presentation/d/1l4PO8_NkQ10fmULIWAaiHB2hoJoOf40gsz3-YTJPxaM/edit?usp=sharing) - Heng Hong
- Industry Updates, [Bitcoin Projects' Fundraising Updates](https://docs.google.com/spreadsheets/d/1k0I348eU-dEVKTTBVwfyyXozLjDCTqCPszLFGgJ7KOg/edit?usp=sharing) - Darius

# Industry Updates - Bitcoin News & Releases

#### [Taproot Assets on Lightning Network](https://x.com/lightning/status/1815768786752164213)

Lightning Labs: "Announcing the first mainnet release of multi-asset Lightning."

With Taproot Assets, users can make instant, low fee asset transfers, bringing trillions of stablecoin volume to Bitcoin.

Lightning is now the global, interoperable financial layer. 🌐

![Global Instant Settled Foreign Exchange|1000](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*H6ZyZ0VqKYH5gSAPkrPVKA.gif)

![Global Instant Settled Foreign Exchange|1000](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*PbiDXu8a4M_F3DhFT6kOjw.gif)

#### [Disclosure of vulnerabilities affecting Bitcoin Core versions before 0.21.x](https://bitcoinops.org/en/newsletters/2024/07/05/#disclosure-of-vulnerabilities-affecting-bitcoin-core-versions-before-0-21-0)

Antoine Poinsot posted to the Bitcoin-Dev mailing list a link to an announcement of 10 vulnerabilites affecting versions of Bitcoin Core that have been past their end-of-life for almost two years.

Poinsot’s announcement said additional vulnerabilities fixed in Bitcoin Core 22.0 would be announced later this month, and vulnerabilities fixed in 23.0 would follow next month. Vulnerabilities fixed in later versions will be disclosed according to Bitcoin Core’s new disclosure policy as previously discussed.

#### [Final results of OKX consolidation txs](https://x.com/mononautical/status/1799220429090951582)

Why is there a need for consolidation for UTXO (Unspent Transaction Outputs)?

Consolidation refers o the process of combining multiple small UTXOs into a single or multiple large UTXOs for:
- Higher fee efficiency
- Simpler management
- Faster transactions
- Optimizing blockchain space

OKX started a major consolidation operation at a more sensible ~53 sats/vb but since then they've been gradually bidding against themselves, until at a current point they're trying to pay 350+ s/vb.

Total confirmed and pending OKX consolidations starting from block 846867:
- 2,385 transactions
- 357,092 inputs
- Consumed 103 MvB
- Average fee rate of 246.65 sat/vB
- Total fees of 254.28 BTC (approx. $17.6 million)
- Recouped 1,738.26 BTC

OKX probably paid 5-8x what they needed to.

#### [Miners keeping list of compromised addresses to increase fees?](https://x.com/mononautical/status/1800496416252743919)

1. The attacker monitors the blockchain for transactions sent to known low-entropy addresses that they suspect might be targeted by others.
2. When they see funds sent to one of these addresses, they quickly create a new transaction that attempts to send those same funds to a different address (likely one they control).
3. This is essentially an attempt at a double-spend. The attacker is racing against the original recipient to move the funds first.
4. By attaching extremely high fees to their transaction, the attacker increases the likelihood that miners will include their transaction in the next block, potentially beating out the legitimate recipient's attempt to move the funds.
5. This attack exploits the window between when a transaction is broadcast and when it's confirmed in a block.

Sender just lost half a bitcoin to fees after withdrawing to a compromised address.

#### [Concurrently secure blind signatures](https://eprint.iacr.org/2022/1676)

In order for a blind signing service to scale up to anything like internet scale, it must be able to service requests concurrently. The original blind signing protocol and many variants are vulnerable to an attack where the attacker opens many sessions and ends up being able to forge a signature on a chosen message.

#### [Silent Payments](https://silentpayments.xyz/docs/)

Silent payments are a technique for generating addresses for a user non-interactively, which can still be detected by wallet software.

Silent Payments make receiving Bitcoin easier than ever while preserving your privacy.

Silent Payments allow you to create a single, static address to share with friends, use for donations, or post for tips without sacrificing privacy. When someone wants to send you a payment, they use the unique public key that is a part of your Silent Payment address, combine it with the public keys of the outputs they want to send and generate a unique, one-time address that looks on-chain just like any other Taproot address. The main advantages of Silent Payments are:

 Simpler user experience: users just need to worry about a single, static address instead of the hurdles of generating new addresses for every receive.
 Better receiver privacy: address re-use with Silent Payments is impossible, as no two senders can generate the same on-chain address.
 Better sender privacy: receivers have no way of connecting sends from the same receiver together, providing better privacy for even the sender.
 No server required: anyone with a wallet supporting Silent Payments can receive funds without address reuse, without communication, and without running complex infrastructure.
 
#### [FROST Updates](https://github.com/BlockstreamResearch/bip-frost-dkg)

The FROST (Flexible Round-Optimal Schnorr Threshold) signature scheme is designed to address key challenges in cryptographic signing processes by allowing a group of participants to jointly create a Schnorr signature using a threshold scheme. Areas of application: multi-sig wallets, DeFi whereby FROST are integrated into smart contracts to manage access controls, etc.

ChillDKG is proposed as an enhancement to the existing PedPop DKG (Distributed Key Generation) protocol for use with FROST. Here’s what ChillDKG brings to the table:

- Traditional PedPop DKG requires secure communication channels and external consensus mechanisms. ChillDKG incorporates these elements directly into the protocol, eliminating the need for external dependencies. This "batteries included" approach makes it easier to implement and deploy.
- ChillDKG provides minimal but sufficient implementations for **secure channel communication** between participants. This ensures that secret shares are protected from eavesdropping and tampering.
- The protocol includes a **built-in consensus mechanism** to ensure that all participants agree on the protocol's results. This prevents scenarios where some participants might end up with invalid or inconsistent secret shares, thus avoiding catastrophic failures.

