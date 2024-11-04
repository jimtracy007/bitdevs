---
layout: post
type: socratic
title: "Socratic Seminar 4"
---


We will start with introductions, some basic ground rules, and jump into technical discussions. We will cover aspects of the bitcoin protocol, new research developments, recent news, and software developments.
# Announcements
-   No pictures or recordings
-   Thank you sponsors:
	- [OGBC](https://www.ogbc.com/) - for the event space
	- [Lightning Ventures](https://ltng.ventures/)  for the pizzas
	- [APRO Oracle](https://www.apro.com/) for the happy hour at [Joo Bar](https://www.joo-bar.com/)
- Our website is up! https://bitdevs.sg/  
- Introductions
	# Agenda
	- Pizza + Networking
	- Bitcoin Basics - [UTXO vs Account Model](https://docs.google.com/presentation/d/1lH4c6VV399MZUKbOFf-UbhT_5nR4aQERH8TOu_SUaIA/edit?usp=sharing), [Heng Hong](https://x.com/HengHongLee)
	- Industry Updates - Bitcoin News, Releases & [Fundraising Updates](https://docs.google.com/spreadsheets/d/1k0I348eU-dEVKTTBVwfyyXozLjDCTqCPszLFGgJ7KOg/edit?gid=0#gid=0), Darius
	- Resistance Money - A Philosophical Case for Bitcoin, [Andrew Bailey](https://x.com/resistancemoney)
	
# Industry Updates - Bitcoin News & Releases

#### [Vulnerabilities disclosed for LND versions prior to 0.17, and other Denial-of-Service attacks](https://delvingbitcoin.org/t/estimating-likelihood-for-lightning-payments-to-be-in-feasible/973)
Matt Morehouse publicly disclosed DoS vulnerabilities for Lightning Network implementations after privately disclosing to Lightning Labs about a year ago.

DoS vulnerabilities are critical because when Lightning Nodes are taken offline, funds are at risk

Morehouse describes other methods of DoS attacks which affects CLN and eclair implementations too. Also does a great job highlighting the difference in how implementations handle being offline

As more features are added to the Lightning Network Protocol, then attack surfaces increases from the additional complexity. Encourages more awareness into security research on lightning

#### [**Bitcoin Core v27.1: Bug Fixes & Performance Improvements**](https://bitcoincore.org/bin/bitcoin-core-27.1/)
Another release of Core

“Bitcoin Core version v27.1 is now available from: https://bitcoincore.org/bin/bitcoin-core-27.1/ or through BitTorrent (magnet link),” announced the project. “This release includes various bug fixes and performance improvements, as well as updated translations. Please report bugs using the issue tracker at GitHub.”

#### [Bitcoin Optech #306: Disclosure of Vulnerabilities Affecting Older Versions of Bitcoin Core](https://bitcoinops.org/en/newsletters/2024/06/07/)
This issue announces an upcoming disclosure of Bitcoin Core vulnerabilities, and describes: a draft BIP for testnet4, functional encryption covenants, proposed updates to 64-bit arithmetic in Bitcoin Script, looks at OP_CAT script to validate proof of work, as well as proposed update to BIP21.

“Several members of the Bitcoin Core project discussed on IRC a proposed policy for disclosing vulnerabilities that affected older versions of Bitcoin Core…” “After this policy has a chance to be further discussed, it is the intention of the project to begin disclosing vulnerabilities affecting Bitcoin Core 24.x and below. It is strongly recommended that all users and administrators upgrade to Bitcoin Core 25.0 or above within the next two weeks.”