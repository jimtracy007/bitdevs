---
layout: post
type: socratic
title: "Socratic Seminar 3"
---

We will start with introductions, some basic ground rules, and jump into technical discussions. We will cover aspects of the bitcoin protocol, new research developments, recent news, and software developments.
# Announcements
-   No pictures or recordings
-   Thank you [OGBC](https://www.ogbc.com/) and [Lnfi Network](https://www.lnfi.network) for sponsorship of the event space and pizzas
-   Introductions
# Agenda
- Prelude on Covenants - Darius
- Industry Updates - Darius
- Covenants and Restoring Scripts - Brandon (video conf)

# <u>Prelude on Covenants</u>
### Limited expressivity in Bitcoin Scripts
#### What it can do:
- Rearrange the stack, check for equality, branch on stack values
- Limited arithmetic 32-bit numbers: add and substract
- Hash and check ECDSA/Schnorr signatures
#### What it cannot do:
- No **loops**, goto, recursion (not turing-complete)
- No **bitwise** operations
- No arithmetic opcodes to do multiplication or division
- No **concatenation** of elements on the stack
- No way to **introspect** transactions (e.g. cannot access transaction amounts), or to carry over state from tx output to tx output
#### History
- We used to have some of these opcodes but its disabled by Satoshi back in 2010 due to inherent vulnerabilities. One of which is OP_CAT which concatenates two stack elements could have been used to remotely crash a Bitcoin node (resource exhaustion attacks).

#### What are Covenants?
- **Covenants** enforce constraints on how transactions (UTXOs) are spent in the future, extending the existing Bitcoin Script capabilities
- It opens up this capability of creating huge and complex contracts directly on Bitcoin with most this logic being pre-computed off-chain only executing when needing to on the chain. 
- This allows users to commit a set of pre-defined spending conditions that we can then create crazy things like payment trees, payment pools and smart vaults and these improve Bitcoin's, Lightning's and UTXO's scalability and privacy.

# 3 Key Covenants Proposal
#### OP_CHECKTEMPLATEVERIFY (CTV) (BIP-119)
- If you are using CTV your transaction must have a template that matches the previously set template, the spending conditions must be met. Once applied these contracts cannot be changed by anyone, they allow for provably trustless sharing of UTXOs.
- **Shared UTXOs** (Lightning Network's channel is already allowing sharing of UTXO between two parties with a 2/2 multi-sig)
	- Payment Tree and Payment Pools
	- **Channel factory** (batched channels) with m-out-of-n multi-sig channel
	- Without shared UTXOs, the planet just doesn't have enough computing power for everybody to have their own UTXOs. This also comes with privacy benefits, as many people will share a single footprint on L1. 

#### OP_CAT (BIP-420)
- Takes two pieces of data on the stack and adds them together to form one.
- Not that harmful in today's context there is a stack limit of 520 bytes
- 10 lines of code implementation
- [ 0xBE, 0xEF ] --> [ 0xBEEF ]
- Can be used for:
	- Arithmetic opcodes to do multiplication or division
	- Transaction introspection and state (e.g. access transaction amounts in script)
	- Verification of Merkle proof
	- Verify SNARKs in script (but the size still too big to be put into a script)
	- CATVM and Drivechains
	- ... and more

#### LNHANCE = OP_CTV + OP_CSFS + OP_INTERNALKEY
- Proposed by Brandon
- MOST of the code is CTV, but adds just enough to enable additional interesting protocols (e.g. Lightning Symmetry, simplified PTLC scripts, rebindable pre-signed vaults)
- Why LNHANCE? 
	- You get almost everything of CTV, APO, TXHASH and ... many more with much less code and much more guarantee and much more efficiency
	- This combination provides a bit more flexibility and programmability than just OP_CTV alone, and enables extra things such as LN symmetry/eltoo.
- **eltoo (also called LN-Symmetry)** allowing a latest channel state  to replace an earlier channel state without requiring a penalty. Greater efficiency and more friendly: LN nodes using eltoo only need to store the latest state (only a few kilobytes). For certain devices that lack large amounts of persistent storage (for example, hardware wallets), they may not be able to store enough data to effectively use penalty-based LN—but as long as they can store a few kB, they should be able to use eltoo-based LN.

#### Great Script Restoration
 - CTV, APO, LNHANCE, TXHASH, CCV, VAULT, etc. have failed to gain consensus because they are variously framed by both what they do enable and what they deliberately avoid enabling (e.g. CTV and LNHANCE avoid enabling Drivechains, but do enable coinpools and lightning symmetry respectively). This has resulted in in-fighting with everyone wanting to enable their thing, but still trying to avoid enabling some other thing. Two things that everyone wants to avoid enabling:
	 - avoid enabling turing complete scripts (to ensure that we can statically analyze all scripts)
	- avoid enabling anything that can increase validation costs for nodes
 - Nothing is comprehensive enough for anyone to think, outside of the proposal originator, that it is the sensible next move forward.
 - This is where the [**Great Script Restoration**](https://bitcoinmagazine.com/technical/the-great-script-restoration-a-path-forward-for-bitcoin) comes in. Its a toolkit for developers to advocating for and analyzing the comprehensive restoration of scripts, just as Satoshi Nakamoto originally designed, we can actually attempt to explore the entire functional space we need, rather than debating and quarreling over which small feature extension is good enough for now.
 - How we improve bitcoin is by engineering the best version of script that we can. What this enables is any developer to validate their contracts on bitcoin as long as they don't unfairly burden nodes (i.e. their validation cost per byte doesn't exceed that of checksig).
 - By **varops limit**. The idea is to assign a cost for each of the reactivated opcodes to take into account the worst case, i.e. most expensive computational cost to validate, that each opcode could create. With this, every one of these opcodes would have its own limit of sorts to restrain how many resources it could consume in verification. It would also be based on the size of any transaction using them, so maintain the ease of reasoning about it, while still adding up to an implicit global limit per block.
 - This would solve the denial of service risks that caused Satoshi to disable all of these opcodes in the first place.







# <u>Industry Updates</u>

# Bitcoin
#### The halving
- [**ViaBTC's total reward for the halving block of >74BTC**](https://x.com/mononautical/status/1783528618720727288)
- [**Halving Chaos**](https://jimmysong.medium.com/halving-fee-chaos-4573b3c8bc9f)
	- Runes have resulted in some really high fees
	- Just in the first 18 blocks, there’s been over $20M in fees spent, most of that in Runes issuance.
	- there is a deeper cost in terms of block space congestion, where fees of 1000 sats/vbyte are currently not enough to get into certain blocks.
#### [**Reducing Bitcoin blocksize limit from 4mb to 300kbs**](https://x.com/BTCtoOblivion/status/1794560100926902673)
- Existing blocks are filled with spam --> higher node running costs
- Debate on reducing blocksize limit to 300kbs to:
	- keep cost of running nodes low (more nodes --> high decentralisation)
	- promote scalability in L2s, L3s instead of onchain scaling

#### [BitVMX: A Virtual CPU to optimistically execute arbitrary programs on Bitcoin](https://groups.google.com/g/bitcoindev/c/8IJS0WK_Cp4)
- developed by @RootstockLabs & @FairGateLabs
- an enhanced, pragmatic upgrade to the BitVM computing paradigm that could make BitVM faster, more efficient, and easier for developers to implement. 
- Both BitVM and BitVMX work in a similar way to Optimistic Roll Ups on Ethereum. As in - they allow computation to be ~verified~ on Bitcoin instead of ~executed~ on Bitcoin. This means - no hard forks required. It all works within the constraints that Bitcoin provides.
- The team at RootstockLabs approached this with a simple question: how could we use BitVM to build better bridges to Bitcoin? And do this in a way that is much more computationally efficient, and therefore more suitable for the practical needs of a Bitcoin side chain.
- Where BitVMX enhances BitVM is in the amount of pre-computation required, the transaction space consumed, and more importantly, the complexity of the solution. 
- Now, in theory, BitVMX could be used to run anything on Bitcoin - even Linux. But we expect the most likely and most obvious early use cases are:
	- building aggregator oracles to collect, verify, and store data on Bitcoin
	- running SNARK or STARK verifiers that could be used to store and automate fund distribution on Bitcoin, and
	- Building bridges between Bitcoin and sidechains like Rootstock instead of 2WayPeg

#### [Utreexod beta release](https://groups.google.com/g/bitcoindev/c/5GyV9af9lv4)
- Utreexo is a novel method for managing the Bitcoin UTXO (Unspent Transaction Output) set, designed to make Bitcoin nodes more efficient. The UTXO set is a collection of all unspent transaction outputs, which is crucial for validating transactions in the Bitcoin network. As Bitcoin grows, the UTXO set becomes larger, making it more resource-intensive for nodes to maintain.
- Utreexo introduces a cryptographic accumulator to represent the UTXO set in a compact form. This approach significantly reduces the storage requirements for full nodes. Here’s a breakdown of key concepts:
	- **Accumulator:** A data structure that allows for the compact representation of a large set of data. In Utreexo, it enables the representation of the entire UTXO set in a very small size.
	- **Proofs:** When a transaction spends a UTXO, it provides a proof that the UTXO exists in the accumulator. This proof can be efficiently verified.
	- **Efficiency:** Utreexo reduces the storage requirements for full nodes, making it possible to run a full node on devices with limited storage capacity, like smartphones or small computers.
	- **Improved Deletion Algorithm:** The deletion algorithm in Utreexo allows for efficient removal of spent UTXOs from the accumulator.
- Utreexod, a full node implementation that incorporates Utreexo accumulators. 
	- **Utreexo Accumulators:** Utreexod implements Utreexo accumulators with an improved deletion algorithm. This allows for efficient handling of the UTXO set, reducing storage requirements.
	- **Efficient P2P Transaction Relay:** The node supports efficient peer-to-peer (P2P) transaction relay by caching Utreexo proofs for transactions in the mempool (a pool of pending transactions).
	- **Quick Sync:** Utreexod includes a feature called AssumeUtreexo which allows for rapid synchronization to the current tip of the blockchain.
	- **Built-in Wallet Support:** The node comes with built-in wallet support via the Bitcoin Development Kit (BDK), facilitating seamless wallet operations.
	- **Electrum Personal Server Support:** This feature allows Utreexod to work with other wallets that use Electrum, a popular lightweight Bitcoin wallet.
#### [DLCs vs L2 vs Arch's native zkVM](https://x.com/bonomat/status/1790319800650186831)
- DLC Oracle **can be decentralised** by having a quorum of oracles
- DLCs **support all tokens** (native/non-native) as DLCs work with UTXOs and anything that can be transferred in an UTXO can be used in a DLC.
- DLC has **less programmability** than L2 or Arch which can run a turing complete VM
- DLCs can be **composed**: you can spend one DLC into another. Using @get10101's DLC-channels you can even settle all of the DLCs off-chain without having to go on-chain. 
- DLCs are permissionless as no one's permission is required. DLCs are discreet in the sense that no external observer can learn its existence or details from the public ledger.
#### [MicroStrategy Orange](https://bitcoinmagazine.com/business/microstrategy-announces-decentralized-id-platform-on-bitcoin-called-microstrategy-orange)
- MicroStrategy Orange, which is an an enterprise platform for building decentralized identity applications on the Bitcoin blockchain
- "Custodial or non-custodial, the obvious thing is every Bitcoin wallet out there should incorporate the capability of creating a Bitcoin based digital identity. Many messaging platforms suffer from the same challenges that email does. When you get a text message, how do you know the person who sent you the text message...We would want to include an orange check for these different messaging platforms." 
- "Where I can now credential my identity anchored to Bitcoin — with a university degree, with a course certification issued by a hyperscaler, with your medical record and present those and have those verified all in a decentralized way. But with the ultimate identity living and being anchored to the Bitcoin blockchain."

#### [Alpen - Introducing SNARKnado](https://www.alpenlabs.io/blog/snarknado-practical-round-efficient-snark-verifier-on-bitcoin)
- **BitVM**: Introduced by Robin Linus, BitVM is a permissioned bisection protocol enabling optimistic verification of arbitrary computations on Bitcoin, using a RISC-V virtual machine. However, it has two main shortcomings:
	- Only a predetermined set of parties can initiate a challenge.
	- The bisection protocol can take a long time, potentially over half a year.
- **BitVM2**: Aims to allow permissionless challenging and reduces the number of rounds by not using bisection, but it faces significant practical challenges and high onchain costs.
- SNARKnado replaces the RISC-V abstraction of BitVM with a protocol based on Polynomial Interactive Oracle Proofs (PIOPs), specifically using PLONK, a popular SNARK construction. This results in a more efficient bisection protocol, reducing the number of rounds needed for verification.
- SNARKnado serves as an intermediate protocol between BitVM and BitVM2. While BitVM2 aims for permissionless challenging, SNARKnado’s structure can inform and improve BitVM2 implementations.
- SNARKnado offers over an **eight-fold improvement** in bisection rounds compared to BitVM, estimating the number of rounds to four. However, it **does not support permissionless challenging like BitVM2**.
	
# Lightning
#### [Taproot Assets Channel - First Mainnet Asset Swap](https://x.com/roasbeef/status/1791171395336192174)
![Taproot Assets on Lightning | 1500](https://assets-global.website-files.com/62f8cf6b2a4c4e15363f6493/64a32becc59e13976d8db972_spaces2F-MIzyiDsFtJBYVyhr1nT2Fuploads2F5YUmQvbKYMvannBLtsLt2FGroup2038812-p-1600.png)

- Zane -[$beefbux channel]-> Yara -[$BTC  Channel]-> Carol
- Zane is able to pay Carol's BTC invoice by swapping his $beefbux for $BTC on the fly using the edge RFQ forwarding+negotiation system!
- Asset-to-asset transfer: Carol can receive in any Taproot Assets /  $BTC while Zane can send in any Taproot Assets / $BTC

#### [How to Make Use of BOLT12 in LDK](https://lightningdevkit.org/blog/bolt12-has-arrived/)
Jeff Czyz from Lightning Dev Kit wrote a walkthrough on BOLT12, explaining what it is and how one can make use of it in LDK.

“BOLT12 is a new payment protocol for Lightning that offers enhanced privacy, reusable payment codes, refunds, and much more, all natively over the Lightning Network.” No additional servers are required. This is all possible using new technologies like onion messages and route blinding. BOLT12 specification defines “an offer that can be considered a precursor to an invoice. It contains less data than an invoice and is smaller to display as a QR code. Optionally, it may contain blinded paths—more on that in a moment. Someone scanning an offer sends an invoice request to the intended recipient, who replies with an invoice containing a unique payment hash.”

#### [Announcing LND 0.18: Smarter Sweeps, Inbound Fees, and Improved Scaling! ](https://x.com/lightning/status/1796235510144540835)
- Delay Aware Fee Bumping + Sweeping:
	- Explanation: This feature aims to optimize the process of fee bumping (increasing the transaction fee to ensure timely inclusion in a block) and sweeping (consolidating small outputs into a single transaction).
	- Impact: Reduces the risk of cascading force closures, where a chain reaction of closing channels could occur due to low fees causing delays.
- Prep for the Transaction Version 3 Fee Bumping Policy:
	- Explanation: Preparation for upcoming changes in Bitcoin's transaction version 3, which will include new rules and mechanisms for fee bumping.
	- Impact: Ensures LND is ready for future updates to the Bitcoin protocol, maintaining compatibility and improving fee management.
- Experimental Support for Inbound Fees:
	- Explanation: Introduces a new traffic shaping option that allows nodes to charge fees for receiving payments, not just sending them.
	- Impact: Provides full nodes with a new way to monetize their services and manage traffic more effectively.
- Blinded Paths Forwarding+Sending Support:
	- Explanation: Adds support for blinded paths, a privacy feature that conceals the actual route a payment takes across the network.
	- Impact: Enhances privacy for users by preventing the exposure of the entire payment route, making it harder to trace transactions.
- Improved Pruned Node and Tor Support:
	- Explanation: Enhances compatibility with pruned Bitcoin nodes (nodes that only store a subset of the blockchain) and improves support for Tor, which anonymizes network traffic.
	- Impact: Increases the accessibility and privacy of running an LND node, especially for users with limited storage or who prioritize anonymity.
- First Class Fee Probing (new EstimateRouteFee RPC call):
	- Explanation: Introduces a new RPC (Remote Procedure Call) method to estimate the fees for a specific route before actually sending a payment.
	- Impact: Allows users and applications to better plan and optimize their payments by understanding potential fees upfront.
- New Native SQL Schema for Invoices:
	- Explanation: Implements a new SQL schema for managing invoices within LND, likely replacing the existing storage method.
	- Impact: Enhances the efficiency and scalability of invoice handling, potentially leading to better performance and more robust data management.
# USD Ecash
- [**First tests of USD Ecash. Backed by BTC**](https://x.com/callebtc/status/1777598819355496587)
- [**Live Demo of Sending Ecash via Cashu**](https://www.youtube.com/watch?v=5DdYR1FebZo)
- [**Cashu.me v0.1: Modern Ecash Experience in Your Browser**](https://www.nobsbitcoin.com/cashu-me-v0-1/)
	- Cashu.me is a Chaumian Ecash wallet for Bitcoin built as a progressive web app (PWA). It is free and open-source software released under the MIT license.

