---
layout: post
type: socratic
title: "Socratic Seminar 2"
---

We will start with introductions, some basic ground rules, and jump into technical discussions. We will cover aspects of the bitcoin protocol, new research developments, recent news, and software developments.
# Announcements
-   No pictures or recordings
-   Thank you [OGBC](https://www.ogbc.com/) and [Lnfi Network](https://www.lnfi.network) for sponsorship of the event space and pizzas
-   Introductions
# Agenda
- Taproot Channels - YY
- Industry Updates - Darius
- Discreet Log Contracts - Darius

# Bitcoin

#### [**Stranded: How Bitcoin is saving wasted energy and expanding financial freedom in Africa**](https://bitcoinmagazine.com/check-your-financial-privilege/stranded-bitcoin-saving-wasted-energy-in-africa)
Alex Gladstein has a great article about how Bitcoin Mining greates value for communities by tapping into stranded Energy.

#### [BitVM 8-bit PC](https://x.com/super_testnet/status/1732493052185391160?s=20)
“It’s true, you can write bitcoin smart contracts in Assembly now instead of learning boolean logic circuits,” wrote @Super Testnet. Someone also wrote a multiplication function for this virtual CPU.

#### [UTXO Arbitrage by Magic Eden on Coinbase](https://twitter.com/mononautical/status/1758262223456162279?s=46&t=WMmqJ4MdyeBHjVDNEbJ-rg)
- For every marketplace purchase on Magic Eden, they include a tiny output (UTXO) for the marketplace fee. These are typically tiny outputs, 1000 sats, 2000 sats, etc. A pain in the butt to consolidate some day. Each of these costs money to spend.
- The genius move is that instead of using their own address to receive these utxos, they are using their Coinbase account address. So all these deposits just get credited to Magic Eden's Coinbase account, and Coinbase is responsible for paying the fees to consolidate them.
- Since Magic Eden started using this address in early December '23, Coinbase has paid 65.16 BTC* to consolidate 183,743 inputs worth 140.30 BTC.
- For 110,444 of those (worth 12.30 BTC), the fees paid actually exceeded the value of the inputs, leading to a dead loss of 34 BTC.

#### 64-bit Arithmetic Soft Fork
This BIP (Bitcoin Improvement Proposal) proposes the addition of new arithmetic opcodes to the Bitcoin protocol to enable 64-bit signed integer math operations directly within Bitcoin scripts. These new opcodes include:

OP_ADD64: Adds two 64-bit signed integers.
OP_SUB64: Subtracts one 64-bit signed integer from another.
OP_MUL64: Multiplies two 64-bit signed integers.
OP_DIV64: Divides one 64-bit signed integer by another.
OP_NEG64: Negates a 64-bit signed integer.
OP_LESSTHAN64: Compares two 64-bit signed integers to check if the first is less than the second.
OP_LESSTHANOREQUAL64: Compares two 64-bit signed integers to check if the first is less than or equal to the second.
OP_GREATERTHAN64: Compares two 64-bit signed integers to check if the first is greater than the second.
OP_GREATERTHANOREQUAL64: Compares two 64-bit signed integers to check if the first is greater than or equal to the second.
These opcodes enable more complex arithmetic and comparison operations to be performed within Bitcoin scripts, which can be useful for implementing advanced smart contracts and applications on the Bitcoin blockchain.

Additionally, the BIP proposes a set of conversion opcodes to facilitate the conversion of existing Bitcoin protocol numbers, known as CScriptNums, into 4 and 7-byte little-endian representations. These conversion opcodes include:

OP_SCRIPTNUMTOLE64: Converts a CScriptNum to a 7-byte little-endian representation.
OP_LE64TOSCRIPTNUM: Converts a 7-byte little-endian representation to a CScriptNum.
OP_LE32TOLE64: Converts a 4-byte little-endian representation to a 7-byte little-endian representation.
These conversion opcodes are useful for interoperability between existing Bitcoin protocol numbers and the new 64-bit signed integer arithmetic operations proposed by the BIP. They ensure that data can be converted and manipulated consistently within Bitcoin scripts, maintaining compatibility with existing protocol standards while expanding functionality.

https://github.com/bitcoin/bips/pull/1538
https://github.com/bitcoin/bitcoin/pull/29221
https://delvingbitcoin.org/t/64-bit-arithmetic-soft-fork/397
Credit: Andrew Poelstra, Sanket Kanjalkar, Chris Stewart

#### [Mempool Incentive Compatibility](https://delvingbitcoin.org/t/mempool-incentive-compatibility/553)

The post provides an in-depth exploration of incentive compatibility in the context of Bitcoin mempool management, particularly focusing on the ordering of transactions to maximize fees. It discusses various scenarios, motivations, and challenges associated with selecting transactions for inclusion in blocks, considering factors such as fee rates, block size constraints, topology requirements, and transaction dependencies.

# Lightning

#### [BTCPayServer - LNBank Plugin Exploit](https://stacker.news/items/347361?ref=nobsbitcoin.com)
- BTCPay ServerBTCPay Server is a self-hosted, open-source cryptocurrency payment processor.
- LNbank is a plugin for BTCPay Server to use the internal Lightning node in custodial mode: It allows server admins to open up the Lightning node and give users access via custodial layer 3 wallets. 
- What happened? 
	- From the BTCPay server the attackers were able to send payments to Bitlifi (CEX) wallet using the LNbank API
	- The BTCPay server is connected to victim's LN node, which sent all the payments using different routes/channels but always ending the transaction on CEX's node ln-1.anycoin.cz.
	- The attack stopped at 21:02h UTC when victim understood what was happening and shutdown the servers where BTCPay server and the LN node are running.
	- The final amount of BTC transferred from victim node to CEX node was evaluated to be ~4BTC
	- All funds were then transferred out of CEX node
	- List of accounts created at the BTCPay server and all the information related to the attackers were exposed for CEX's further investigation with the KYC'ed information but they choose not to do anything
- LNbank dev admitted that there is a bug and urge everyone to update to v1.8.9 as soon as possible.
- Note: This is a form of hot wallet risk. Software running on lightning nodes often have full control of the node in question which means any vulnerability found in that software can be used to steal balances. Recommend to be careful of what apps to run, and secure authentication token files properly (LND uses special cookies called macaroons)

#### [Liquidity on Lightning: Moving from UX to Economix ](https://medium.com/breez-technology/liquidity-on-lightning-moving-from-ux-to-economix-6e597d9e1abd)
- By Roy Sheineld, Breez Technology
- The discussion delves into the evolution of liquidity on the Lightning Network, emphasizing the role of Lightning Service Providers (LSPs) in addressing technical challenges and promoting its transition into a thriving financial network. It outlines the constraints and opportunities faced by LSPs, such as volatile on-chain transaction fees, and highlights strategies to enhance their viability, including optimizing the lifetime value to customer acquisition cost ratio (LTV/CAC). The article also explores broader constraints and opportunities for Lightning as a whole, underscoring its potential to significantly increase Bitcoin's throughput while adapting to various parameters and local conditions. Ultimately, it emphasizes the importance of embracing constraints as natural and integral to the maturation process of Lightning and the broader transition to a bitcoin economy.
- Note: Breez offers Lightning as a Service which enables developers to integrate Lightning and bitcoin payments into their apps and services with a very shallow learning curve. The use cases are endless – from social apps that want to integrate tipping between users to fintech services interested in adding bitcoin solutions. Crucially, this SDK is an end-to-end, non-custodial, drop-in solution powered by Greenlight, a built-in liquidity provider, on-chain interoperability, third-party fiat on-ramps, and other services users and operators need.

#### Discrete Log Contracts

Discreet Log Contracts (DLCs) are a type of smart contract protocol that operates on the Bitcoin blockchain, allowing for the execution and settlement of contracts based on events external to the blockchain.

During the setup of a DLC both parties lock up funds in a DLC output. This output can only be spent with signatures from both parties. At the time of the DLC setup the actual event outcome is not know yet, but the oracle has announced that it will attest to its outcome at a specific point in the future. Both parties generate multiple CETs, each defining how to split up the locked up funds based on a specific potential future event outcome. After agreeing on the CETs, both parties exchange adaptor signatures on each of them. An adaptor signature for any given CET will only be unlocked if the oracle attests to the specific event outcome associated with the corresponding CET.

The key characteristics of a CET are the following:

- Only one CET will become valid after the attestation of one or multiple oracles.
- Neither party can publish a valid CET without having the oracle's attestation.
- Publishing a CET is unilateral after the oracle's attestation.
- The most simple example of a DLC is a binary option. The diagram below illustrates the transaction structure of such a DLC. As the potential future event outcomes can only be true or false, only two CETs have to be generated.

![Discrete Log Contracts](https://10101.finance/assets/images/binary_dlc-9ab42cff6edd3fcdd312f8f920328e0c.png)
