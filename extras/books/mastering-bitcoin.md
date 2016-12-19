# Mastering Bitcoin
[main github](https://github.com/bitcoinbook/bitcoinbook/tree/first_edition)

[index](https://github.com/bitcoinbook/bitcoinbook/blob/first_edition/book.asciidoc)

## preface

read the white paper written by Satoshi Nakamoto: "this isn’t money, it’s a decentralized trust network"

 Like an ant colony, the bitcoin network is a resilient network of simple nodes following simple rules that together can do amazing things without any central coordination.

## Ch01 Introduction
### What Is Bitcoin?
Bitcoin is a collection of concepts and technologies that form the basis of a digital money ecosystem.
Bitcoins are created through a process called "mining," which involves competing to find solutions to a mathematical problem(dynamically adjusted) while processing bitcoin transactions. Every 10 minutes on average, someone is able to validate the transactions of the past 10 minutes and is rewarded with brand new bitcoins.

The protocol also halves the rate at which new bitcoins are created every four years. The result is that the number of bitcoins in circulation closely follows an easily predictable curve that reaches 21 million by the year 2140.

Cryptographic digital signatures enable a user to sign a digital asset or transaction proving the ownership of that asset.

Bitcoin represents the culmination of decades of research in cryptography and distributed systems and includes four key innovations brought together in a unique and powerful combination. Bitcoin consists of:

* A decentralized peer-to-peer network (the bitcoin protocol)
* A public transaction ledger (the blockchain)
* A decentralized mathematical and deterministic currency issuance (distributed mining)
* A decentralized transaction verification system (transaction script)

### History of Bitcoin

Bitcoin was invented in 2008. The key innovation was to use a distributed computation system (called a "proof-of-work" algorithm) to conduct a global "election" every 10 minutes, allowing the decentralized network to arrive at consensus about the state of transactions (solves the issue of double-spend and the "Byzantine Generals' Problem" in distributed computing).
The bitcoin network started in 2009. Satoshi Nakamoto withdrew from the public in April of 2011.


### Bitcoin Uses, Users, and Their Stories
* North American low-value retail
* North American high-value retail
* Offshore contract services
* Charitable donations
* Import/export
* Mining for bitcoin

### Getting Started

The three main forms of bitcoin clients are:
* **Full client** stores the entire history of bitcoin transactions,manages the users' wallets, and can initiate transactions directly on the bitcoin network.

* **Lightweight client** stores the user’s wallet but relies on third-party–owned servers for access to the bitcoin transactions and network.

* **Web client** store the user’s wallet on a server owned by a third party.

* **Mobile Bitcoin** can either operate as full clients, lightweight clients, or web clients. Some mobile clients are synchronized with a web or desktop client

#### Quick Start
Example of buy bitcoin from another user, install [Multibit](https://multibit.org/), copy public key, buy bitcoins with dollars, check the transaction.

## Ch02 How Bitcoin Works
Transactions, Blocks, Mining, and the Blockchain

Bitcoin Transactions

Constructing a Transaction

Bitcoin Mining

Mining Transactions in Blocks

Spending the Transaction

## Ch03 The Bitcoin Client
Bitcoin Core: The Reference Implementation

Using Bitcoin Core’s JSON-RPC API from the Command Line

Alternative Clients, Libraries, and Toolkits

## Ch04 Keys, Addresses, Wallets

Introduction
Bitcoin Addresses
Implementing Keys and Addresses in Python
Wallets
Advanced Keys and Addresses
Chapter 5Transactions
Introduction
Transaction Lifecycle
Transaction Structure
Transaction Outputs and Inputs
Transaction Chaining and Orphan Transactions
Transaction Scripts and Script Language
Standard Transactions
Chapter 6The Bitcoin Network
Peer-to-Peer Network Architecture
Nodes Types and Roles
The Extended Bitcoin Network
Network Discovery
Full Nodes
Exchanging “Inventory”
Simplified Payment Verification (SPV) Nodes
Bloom Filters
Bloom Filters and Inventory Updates
Transaction Pools
Alert Messages
Chapter 7The Blockchain
Introduction
Structure of a Block
Block Header
Block Identifiers: Block Header Hash and Block Height
The Genesis Block
Linking Blocks in the Blockchain
Merkle Trees
Merkle Trees and Simplified Payment Verification (SPV)
Chapter 8Mining and Consensus
Introduction
Decentralized Consensus
Independent Verification of Transactions
Mining Nodes
Aggregating Transactions into Blocks
Constructing the Block Header
Mining the Block
Successfully Mining the Block
Validating a New Block
Assembling and Selecting Chains of Blocks
Mining and the Hashing Race
Consensus Attacks
Chapter 9Alternative Chains, Currencies, and Applications
A Taxonomy of Alternative Currencies and Chains
Meta Coin Platforms
Alt Coins
Noncurrency Alt Chains
Future of Currencies
Chapter 10Bitcoin Security
Security Principles
User Security Best Practices
Conclusion
Appendix Transaction Script Language Operators, Constants, and Symbols
Appendix Bitcoin Improvement Proposals
Appendix pycoin, ku, and tx
Appendix Available Commands with sx Tools

ch03.asciidoc

ch04.asciidoc

ch05.asciidoc

ch06.asciidoc

ch07.asciidoc

ch08.asciidoc

ch09.asciidoc

ch10.asciidoc

appdx-scriptops.asciidoc

appdx-bips.asciidoc

appdx-pycoin.asciidoc

appdx-bx.asciidoc

index.asciidoc

colo.asciidoc
