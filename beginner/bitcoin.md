---
description: Rejecting Technological Capitalization to Create a New Human Civilization
icon: bitcoin
---

# 1. Insights from Bitcoin

It looks like that there's only a sort of currency, BTC, in Bitcoin, while Ethereum, five years after Bitcoin's inception, has technically surpassed it with a white paper. Yet for 16 years, Bitcoin has been the dominant token! It has consistently ranked at the top of the blockchain market. On January 7, 2025, Bitcoin's market cap accounted for as much as 56.4%, while the second-ranked ETH's market cap was only 12.3%!&#x20;

Isn't that thought-provoking?



Proof-of-Work (PoW)

So far, very few people have truly understood the concept of Bitcoin's Proof-of-Work (PoW)! Let’s trace back to the source and revisit the [Bitcoin White Paper](https://bitcoin.org/bitcoin.pdf) (Bitcoin: A Peer-to-Peer Electronic Cash System) published by Satoshi Nakamoto sixteen years ago.

> **Abstract**.  A purely peer-to-peer version of electronic cash would allow online payments to be sent directly from one party to another without going through a financial institution. Digital signatures provide part of the solution, but the main benefits are lost if a trusted third party is still required to prevent double-spending. We propose a solution to the double-spending problem using a peer-to-peer network. The network timestamps transactions by hashing them into an ongoing chain of hash-based proof-of-work, forming a record that cannot be changed without redoing the proof-of-work. The longest chain not only serves as proof of the sequence of events witnessed, but proof that it came from the largest pool of CPU power. As long as a majority of CPU power is controlled by nodes that are not cooperating to attack the network, they'll generate the longest chain and outpace attackers. The network itself requires minimal structure. Messages are broadcast on a best effort basis, and nodes can leave and rejoin the network at will, accepting the longest proof-of-work chain as proof of what happened while they were gone.

The abstract describes the most crucial aspect of Bitcoin: Proof-of-Work (PoW). PoW is so important that it runs throughout the entire White Paper. In the very last sentence of the paper, Satoshi Nakamoto refers to PoW as a **consensus mechanism**:

“Any needed rules and incentives can be enforced with this consensus mechanism.”

This statement clearly tells us that PoW is a governance consensus. In Satoshi's design, PoW is composed of two main components: needed rules and incentives. The enforcement implies that PoW is indeed a governance protocol.

As you can see, the White Paper primarily introduces PoW, but unfortunately, it does not provide a clear definition or complete description of PoW itself.

When I searched Proof of work, google listed the first link which introduced it [as this](https://www.investopedia.com/terms/p/proof-work.asp):

> **What Is Proof of Work (PoW)?**&#x20;
>
> Proof of work (PoW) is a blockchain consensus mechanism that requires significant computing effort from a network of devices. The concept was adapted from digital tokens by Hal Finney in 2004 through the idea of "reusable proof of work" using the 160-bit secure hash algorithm 1 (SHA-1).
>
> Following its introduction in 2009, Bitcoin became the first widely adopted application of Finney's PoW idea (Finney was also the recipient of the first bitcoin transaction). Proof of work is also the mechanic used in many other cryptocurrencies.

Let’s see how long and how bad Wikipedia, which is listed in the 2nd place, describes it:

> Proof of work (PoW) is a form of cryptographic proof in which one party (the prover) proves to others (the verifiers) that a certain amount of a specific computational effort has been expended. Verifiers can subsequently confirm this expenditure with minimal effort on their part. The concept was invented by Moni Naor and Cynthia Dwork in 1993 as a way to deter denial-of-service attacks and other service abuses such as spam on a network by requiring some work from a service requester, usually meaning processing time by a computer. The term "proof of work" was first coined and formalized in a 1999 paper by Markus Jakobsson and Ari Juels. The concept was adapted to digital tokens by Hal Finney in 2004 through the idea of "reusable proof of work" using the 160-bit secure hash algorithm 1 (SHA-1).
>
> Proof of work was later popularized by Bitcoin as a foundation for consensus in a permissionless decentralized network, in which miners compete to append blocks and mine new currency, each miner experiencing a success probability proportional to the computational effort expended. PoW and PoS (proof of stake) remain the two best known Sybil deterrence mechanisms. In the context of cryptocurrencies they are the most common mechanisms.
>
> A key feature of proof-of-work schemes is their asymmetry: the work – the computation – must be moderately hard (yet feasible) on the prover or requester side but easy to check for the verifier or service provider. This idea is also known as a CPU cost function, client puzzle, computational puzzle, or CPU pricing function. Another common feature is built-in incentive-structures that reward allocating computational capacity to the network with value in the form of cryptocurrency.
>
> The purpose of proof-of-work algorithms is not proving that certain work was carried out or that a computational puzzle was "solved", but deterring manipulation of data by establishing large energy and hardware-control requirements to be able to do so. Proof-of-work systems have been criticized by environmentalists for their energy consumption.

So, even more regrettably, Wikipedia and other easily accessible online sources have intentionally or unintentionally seized on the omission by Satoshi, ignored the last sentence of the White Paper, and the special description of the reward mechanism for PoW in the sixth section of the White Paper. These sources tend to emphasize how the work is done but not the necessity and significance of built-in incentive-structures. In a highly developed capitalist era, the concept of "proof of work" is emphasized as the idea that you must work hard (to earn money), which is indeed quite reasonable. It's a pity that if Satoshi had named this consensus mechanism “Proof-of-Reward,” highlighting the importance of rewards in the system, I think the trajectory of blockchain development might have been vastly different!

In my textbook, I describe it this way:\
Proof-of-Work is a governance consensus driven by rewards and centered on the core task of bookkeeping.

> 6. **Incentive**
>
> By convention, the first transaction in a block is a special transaction that starts a new coin owned by the creator of the block. This adds an incentive for nodes to support the network, and provides a way to initially distribute coins into circulation, since there is no central authority to issue them. The steady addition of a constant of amount of new coins is analogous to gold miners expending resources to add gold to circulation. In our case, it is CPU time and electricity that is expended.&#x20;
>
> The incentive can also be funded with transaction fees. If the output value of a transaction is less than its input value, the difference is a transaction fee that is added to the incentive value of the block containing the transaction. Once a predetermined number of coins have entered circulation, the incentive can transition entirely to transaction fees and be completely inflation free.&#x20;
>
> The incentive may help encourage nodes to stay honest. If a greedy attacker is able to assemble more CPU power than all the honest nodes, he would have to choose between using it to defraud people by stealing back his payments, or using it to generate new coins. He ought to find it more profitable to play by the rules, such rules that favour him with more new coins than everyone else combined, than to undermine the system and the validity of his own wealth.

To reiterate, the key to the success of Bitcoin's PoW is its reward mechanism. From the content of the sixth section, we know that Bitcoin provides miners with a dual reward mechanism: the 2nd paragraph tells us, all transaction fees from transfers are rewarded to miners. These fees are not claimed by Satoshi, which surpasses all centralized exchanges. However, this reward fundamentally comes from user contributions. The second reward, described in the first paragrah, is that Bitcoin gradually issues BTC to miners as a reward. The total issuance of BTC is capped at 21 million, and each one is slowly issued as a reward to miners by the public chain Bitcoin—this is the shocking “secret” of Bitcoin's PoW that capitalists have obscured for over a decade!

Indeed, all 21 million BTC were issued as rewards to miners through the system. This design is the crowning touch of PoW! I believe Satoshi’s primary intention was to remove the capitalist stench from BTC's issuance by adopting a perfectly rational approach. This design, viewed from a slightly different perspective, would astound many: the 21 million BTC designed by Satoshi are essentially a public reward fund of Bitcoin's PoW!

Now, here's the key point: In the design of PoW, Satoshi rejected the typical capitalist approach of promising a big pie with one hand (Bitcoin, often nicknamed the "big cake" by Chinese) while capitalizing on technology with the other. Technological capitalization refers to the practice of pre-selling “electronic cash” BTC as capital, allowing more capitalists to gain future anticipated value. By rejecting technological capitalization, Satoshi fundamentally rejected the traditional capitalist mode of production.

Therefore, Satoshi’s design of Bitcoin’s public reward fund carries profound implications! It ultimately inspires us that, guided by the governance consensus of Bitcoin’s PoW, what is being built is not a capitalist future. And it leads to a new civilization for humanity!

Please note that there are two major fundamental issues in the design of Bitcoin! These determine that the Bitcoin cannot succeed.

The first major problem is that Bitcoin does not have the concept of accounts. To determine how many bitcoins a person owns, it is not possible to check from a single account; one must know the total sum of all UTXOs (Unspent TX Output) controlled by that person. This is obviously designed this way to protect so-called personal asset privacy. But we know that accounts are the cornerstone of finance. The lack of an account concept inevitably meant that after the advent of Ethereum, Bitcoin missed the great innovation of smart contracts introduced by the latter.

The second major problem is perfectly reflected in the name of Bitcoin’s consensus. Proof of Work undoubtedly remains rooted in the labor civilization, that is, the labor value theory of capitalism. It is destined to be abandoned by humanity’s new civilization.

However, we can also say that Bitcoin succeeded the very day it generated its first block!

In short, what Bitcoin brings us is some part of its governance philosophy, and some of its technologies. Satoshi Nakamoto's silent departure was a mark of achievement.\
