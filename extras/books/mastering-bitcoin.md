# Mastering Bitcoin
[main github](https://github.com/bitcoinbook/bitcoinbook/tree/first_edition)

[main index](https://github.com/bitcoinbook/bitcoinbook/blob/first_edition/book.asciidoc)

## index

[Ch01 Introduction](#ch01-ntroduction)  
[Ch02 How Bitcoin Works](#ch02-how-bitcoin-works)  
[Ch03 The Bitcoin Client](#ch03-the-bitcoin-client)  
[Ch04 Keys, Addresses, Wallets](#ch04-keys,-addresses,-wallets)

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
Example buy bitcoin from another user, install [Multibit](https://multibit.org/), copy public key, buy bitcoins with dollars, check the transaction in the blockchain.


## Ch02 How Bitcoin Works

### Transactions, Blocks, Mining, and the Blockchain

The bitcoin system, unlike traditional banking and payment systems, is based on de-centralized trust.

A transaction becomes "trusted" and accepted by the bitcoin mechanism of distributed consensus and then recorded on the blockchain.

A blockchain explorer is a web application that operates as a bitcoin search engine, in that it allows you to search for addresses, transactions, and blocks and see the relationships and flows between them.
Popular blockchain explorers include:

* [Blockchain info](https://blockchain.info/)
* [Bitcoin Block Explorer](https://blockexplorer.com/)
* [insight](https://insight.bitpay.com/)
* [blockr Block Reader](http://blockr.io/)

#### Bitcoin Overview
The bitcoin system consists of users with wallets containing keys, transactions that are propagated across the network, and miners who produce (through competitive computation) the consensus blockchain, which is the authoritative ledger of all transactions.

#### Buying a Cup of Coffee

```
Total:
$1.50 USD
0.015 BTC
```
> shows a QR code to be used with a wallet

The payment request QR code encodes the following URL, defined in [BIP0021](https://github.com/bitcoin/bips/blob/master/bip-0021.mediawiki):
```
bitcoin:1GdK9UzpHBzqzX2A9JFP3Di4weBwqgmoQA?
amount=0.015&
label=Bob%27s%20Cafe&
message=Purchase%20at%20Bob%27s%20Cafe

Components of the URL
A bitcoin address: "1GdK9UzpHBzqzX2A9JFP3Di4weBwqgmoQA"
The payment amount: "0.015"
A label for the recipient address: "Bob's Cafe"
A description for the payment: "Purchase at Bob's Cafe"
```
Bob says, "That’s one-dollar-fifty, or fifteen millibits."  
Alice uses her smartphone to scan the barcode on display and authorize the payment.

### Bitcoin Transactions

In simple terms, a transaction tells the network that the owner of a number of bitcoins has authorized the transfer of some of those bitcoins to another owner. The new owner can now spend these bitcoins by creating another transaction that authorizes transfer to another owner, and so on, in a chain of ownership.

Transactions are like lines in a double-entry bookkeeping ledger. In simple terms, each transaction contains one or more "inputs," which are debits against a bitcoin account. On the other side of the transaction, there are one or more "outputs," which are credits added to a bitcoin account. The inputs and outputs (debits and credits) do not necessarily add up to the same amount. Instead, outputs add up to slightly less than inputs and the difference represents an implied "transaction fee," which is a small payment collected by the miner who includes the transaction in the ledger.

In bitcoin terms, "spending" is signing a transaction that transfers value from a previous transaction over to a new owner identified by a bitcoin address.
> The destination key is called an **encumbrance**.

#### Common Transaction Forms

The most common form of transaction is a **simple payment** from one address to another, which often includes some "change" returned to the original owner. This type of transaction has one input and two outputs.

Another common form of transaction is one that aggregates several inputs into a single output (wallets): **Aggregation**.

Finally, another transaction form that is seen often on the bitcoin ledger is a transaction that distributes one input to multiple outputs representing multiple recipients (payroll payments to multiple employees): **Distributing**

### Constructing a Transaction

Alice only needs to specify a destination and an amount and the rest happens in the wallet application without her seeing the details (also works offline).

#### Getting the Right Inputs

Alice’s wallet application will first have to find inputs that can pay for the amount she wants to send to Bob. Most wallet applications keep a small database of "unspent transaction outputs" that are locked (encumbered) with the wallet’s own keys.
This allows a wallet to construct transaction inputs as well as quickly verify incoming transactions as having correct inputs. However, because a full-index client takes up a lot of disk space, most user wallets run "lightweight" clients that track only the user’s own unspent outputs.
If the wallet application does not maintain a copy of unspent transaction outputs, it can query the bitcoin network to retrieve this information (API http get).

Example 1. Look up all the unspent outputs for Alice’s bitcoin address:
``` js
$ curl https://blockchain.info/unspent?active=1Cdid9KFAaatwczBwBttQcwXYCpvK8h7FK

{
	"unspent_outputs":[
		{
			"tx_hash":"186f9f998a5...2836dd734d2804fe65fa35779",
			"tx_index":104810202,
			"tx_output_n": 0,
			"script":"76a9147f9b1a7fb68d60c536c2fd8aeaa53a8f3cc025a888ac",
			"value": 10000000, //0.10 bitcoin
			"value_hex": "00989680",
			"confirmations":0
		}
	]
}
```
[View the transaction from Joe to Alice](https://blockchain.info/tx/7957a35fe64f80d234d76d83a2a8f1a0d8149a41d81de548f0a65a8a999f6f18)

As you can see, Alice’s wallet contains enough bitcoins in a single unspent output to pay for the cup of coffee. Had this not been the case, Alice’s wallet application might have to "rummage" through a pile of smaller unspent outputs

#### Creating the Outputs
A transaction output is created in the form of a script that creates an encumbrance on the value and can only be redeemed by the introduction of a solution to the script. In simpler terms, Alice’s transaction output will contain a script that says something like, "This output is payable to whoever can present a signature from the key corresponding to Bob’s public address." Because only Bob has the wallet with the keys corresponding to that address, only Bob’s wallet can present such a signature to redeem this output. Alice will therefore "encumber" the output value with a demand for a signature from Bob.

This transaction will also include a second output, because Alice’s funds are in the form of a 0.10 BTC output, too much money for the 0.015 BTC cup of coffee. Alice will need 0.085 BTC in change. Alice’s change payment is created by Alice’s wallet in the very same transaction as the payment to Bob.
There will be 0.0005 BTC (half a millibitcoin) left over as transaction fee.

> **transaction fee** is collected by the miner as a fee for including the transaction in a block and putting it on the blockchain ledger.

[View the transaction from Alice to Bob’s Cafe.](https://blockchain.info/tx/0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2)

#### Adding the Transaction to the Ledger

The transaction created by Alice’s wallet application is 258 bytes long and contains everything necessary to confirm ownership of the funds and assign new owners. Now, the transaction must be transmitted to the bitcoin network where it will become part of the distributed ledger (the blockchain).

The purpose of the bitcoin network is to propagate transactions and blocks to all participants. Any bitcoin network node (other client) that receives a valid transaction it has not seen before will immediately forward it to other nodes to which it is connected. Thus, the transaction rapidly propagates out across the peer-to-peer network, reaching a large percentage of the nodes within a few seconds.

Bob’s wallet will immediately identify Alice’s transaction as an incoming payment because it contains outputs redeemable by Bob’s keys. At this point Bob can assume, with little risk, that the transaction will shortly be included in a block and confirmed.

### Bitcoin Mining

The transaction is now propagated on the bitcoin network. It does not become part of the shared ledger (the blockchain) until it is verified and included in a block by a process called mining.

The bitcoin system of trust is based on computation. Transactions are bundled into blocks, which require an enormous amount of computation to prove, but only a small amount of computation to verify as proven. The mining process serves two purposes in bitcoin:

* Mining creates new bitcoins in each block, almost like a central bank printing new money. The amount of bitcoin created per block is fixed and diminishes with time.

* Mining creates trust by ensuring that transactions are only confirmed if enough computational power was devoted to the block that contains them. More blocks mean more computation, which means more trust.

### Mining Transactions in Blocks

A transaction transmitted across the network is not verified until it becomes part of the global distributed ledger, the blockchain. Every 10 minutes on average, miners generate a new block that contains all the transactions since the last block.
New transactions are constantly flowing into the network from user wallets and other applications. As these are seen by the bitcoin network nodes, they get added to a temporary pool of unverified transactions maintained by each node. As miners build a new block, they add unverified transactions from this pool to a new block and then attempt to solve a very hard problem (a.k.a., proof of work) to prove the validity of that new block.

Transactions are added to the new block, prioritized by the highest-fee transactions first and a few other criteria. Each miner starts the process of mining a new block of transactions as soon as he receives the previous block from the network, knowing he has lost that previous round of competition. He immediately creates a new block, fills it with transactions and the fingerprint of the previous block, and starts calculating the proof of work for the new block. Each miner includes a special transaction in his block, one that pays his own bitcoin address a reward of newly created bitcoins.

Approximately five minutes after the transaction was first transmitted by Alice’s wallet, Jing’s ASIC miner found a solution for the block and published it as block #277316, containing 419 other transactions. Jing’s ASIC miner published the new block on the bitcoin network, where other miners validated it and started the race to generate the next block.

[You can see the block that includes Alice’s transaction.](https://blockchain.info/block-height/277316)

A few minutes later, a new block, #277317, is mined by another miner. Because this new block is based on the previous block (#277316) that contained Alice’s transaction, it added even more computation on top of that block, thereby strengthening the trust in those transactions.

By convention, any block with more than six confirmations is considered irrevocable, because it would require an immense amount of computation to invalidate and recalculate six blocks.

### Spending the Transaction

Now that Alice’s transaction has been embedded in the blockchain as part of a block, it is part of the distributed ledger of bitcoin and visible to all bitcoin applications. Each bitcoin client can independently verify the transaction as valid and spendable. Full-index clients can track the source of the funds. Lightweight clients can do what is called a simplified payment verification.

Bob can now spend the output from this and other transactions, by creating his own transactions that reference these outputs as their inputs and assign them new ownership.


## Ch03 The Bitcoin Client

### Bitcoin Core: The Reference Implementation

You can download the reference client [Bitcoin Core](https://bitcoin.org/en/bitcoin-core/), also known as the "Satoshi client,". The reference client implements all aspects of the bitcoin system, including wallets, a transaction verification engine with a full copy of the entire transaction ledger (blockchain), and a full network node in the peer-to-peer bitcoin network.

Check another wallets from [Choose your Bitcoin wallet](https://bitcoin.org/en/choose-your-wallet)

#### Running Bitcoin Core for the First Time
it will download a copy of the transaction ledger (blockchain), in late 2016 100 GB size.

#### Compiling Bitcoin Core from the Source Code
clone the [bitcoin github](https://github.com/bitcoin/bitcoin) and compile the desire release (tag). Assuming the prerequisites are installed, you start the build process by generating a set of build scripts using the autogen.sh script.

You can confirm that bitcoin is correctly installed by asking the system for the path of the two executables, as follows:
`$ which bitcoind` and `$ which bitcoin-cli`

Edit the configuration file `.bitcoin/bitcoin.conf` and enter a username and password.

Run bitcoind in the background with the option -daemon: `$ bitcoind -daemon`

### Using Bitcoin Core’s JSON-RPC API from the Command Line

The Bitcoin Core client implements a JSON-RPC interface that can also be accessed using the command-line helper bitcoin-cli.
`$ bitcoin-cli help`

* Getting Information on the Bitcoin Core Client Status
`$ bitcoin-cli getinfo`

* Wallet Setup and Encryption
  - `$ bitcoin-cli encryptwallet foo`, where foo is the password
  - `$ bitcoin-cli walletpassphrase foo 360`, where 360 is the number of seconds until the wallet is locked again automatically

* Wallet Backup, Plain-text Dump, and Restore
  - `$ bitcoin-cli backupwallet wallet.backup`, creates the backup file.
  - `$ bitcoin-cli importwallet wallet.backup`, restore the backup file.
  - `$ bitcoin-cli dumpwallet wallet.txt`, dump the wallet into a text file.

* Wallet Addresses and Receiving Transactions
  - `$ bitcoin-cli getnewaddress`, result: *1hvzSofGwT8cjb8JU7nBsCSfEVQX5u9CL*. The bitcoin reference client maintains a pool of addresses (size: `keypoolsize` from getinfo). These addresses are generated automatically and can then be used as public receiving addresses or change addresses.
  - `$ bitcoin-cli getreceivedbyaddress 1hvzSofGwT8cjb8JU7nBsCSfEVQX5u9CL 0`, result: *0.05000000*. If we omit the zero from the end of this command, we will only see the amounts that have at least minconf confirmations
 - `$ bitcoin-cli listtransactions`, result *[{tr}]*. Transactions received by the entire wallet.
 - `$ bitcoin-cli getaddressesbyaccount ""` result *[addr]*. List all addresses in the entire wallet.
 - `$ bitcoin-cli getbalance`, result *0.05000000*. Total balance of the wallet. If the transaction has not yet confirmed, the balance returned by getbalance will be zero

* Exploring and Decoding Transactions
  - `$ bitcoin-cli gettransaction 9ca8f969bd3ef5ec2a8685660fdbf7a8bd365524c2e1fc66c309acbae2c14ae3`, result *{tr}*. Transaction hash is **txid**, is not authoritative until a transaction has been confirmed, can be modified prior to confirmation in a block.
  - `$ bitcoin-cli getrawtransaction 9ca8f969bd3ef5ec2a8685660fdbf7a8bd365524c2e1fc66c309acbae2c14ae3`, result *raw hex*. Takes the transaction hash (txid) as a parameter and returns the full transaction as a "raw" hex string, exactly as it exists on the bitcoin network.
  - `$ bitcoin-cli decoderawtransaction 010...000`, result *{tr}*. To get the full contents interpreted as a JSON data structure, the input is previous *raw hex*

  Once the transaction we received has been confirmed by inclusion in a block, the `gettransaction` command will return additional information, showing the `blockhash` (identifier) in which the transaction was included, and the `blockindex`, indicates number of transaction in the block.
  By default, Bitcoin Core builds a database containing only the transactions related to the user’s wallet. To change this set `txindex=1` from ` .bitcoin/bitcoin.conf` and restart bitcoind.

* Exploring Blocks
  - `$ bitcoin-cli getblock 000000000000000051d2e759c63a26e247f185ecb7926ed7a6624bc31c2a717b true`, result *{block}*. The block contains 367 transactions and as you can see, the 18th transaction listed (9ca8f9...) is the txid of the one crediting 50 millibits to our address. The `height` entry tells us this is the 286384th block in the blockchain.
  - `$ bitcoin-cli getblockhash 0`, result *hash*, retrieves a block by its block height.

* Creating, Signing, and Submitting Transactions Based on Unspent Outputs
  - `$ bitcoin-cli listunspent`, result *[{output}]*. Show all the unspent confirmed outputs in our wallet. Bitcoin’s transactions are based on the concept of spending "outputs," which are the result of previous transactions.
  - `$ bitcoin-cli gettxout 9ca8f969bd3ef5ec2a8685660fdbf7a8bd365524c2e1fc66c309acbae2c14ae3 0`, result *{trans}*. Get the details of this unspent output: parameters **txid** and **vout**. To spend this output we will create a new transaction (to a new address `getnewaddress`). We will send 25 millibits to the new address (from a 50 total) and get back change (24.5 mBTC), 0.5 millibits will be transaction fee.
  - `$ bitcoin-cli createrawtransaction '[{"txid","vout"}]' '{"newAddr": 0.025, "oldAddr": 0.0245}'`, result transaction in `hex raw`. To confirm everything is correct use `decoderawtransaction`. The transaction contains an empty **scriptSig** because we haven’t signed it yet. Without a signature, this transaction is meaningless; we haven’t yet proven that we own the address from which the unspent output is sourced.
  - `$ bitcoin-cli walletpassphrase foo 360`, result: unlock wallet  
  `$ bitcoin-cli signrawtransaction rawhextransaction`, result: hex raw, check content with `decoderawtransaction`
  - `$ bitcoin-cli sendrawtransaction rawhextransactionSigned`, returns a transaction hash (txid). Submit the newly created transaction to the network. We can now query that transaction ID with `gettransaction`

### Alternative Clients, Libraries, and Toolkits
* [libbitcoin](https://github.com/libbitcoin/libbitcoin) Bitcoin Cross-Platform C++ Development Toolkit.
* [pycoin](https://github.com/richardkiss/pycoin) Another Python bitcoin library.
* [btcd](https://github.com/btcsuite/btcd) A Go language full-node bitcoin client.
* more (to old, missed javascript options)

## Ch04 Keys, Addresses, Wallets

### Introduction
Ownership of bitcoin is established through digital keys, bitcoin addresses, and digital signatures. The digital keys are not actually stored in the network, but are instead created and stored by users in a file, or simple database, called a **wallet**. Every bitcoin transaction requires a valid signature to be included in the blockchain.
Keys come in pairs consisting of a private (secret) key and a public key.

In the payment portion of a bitcoin transaction, the recipient’s public key is represented by its digital fingerprint, called a bitcoin **address** (represents public key or script)

#### Public Key Cryptography and Cryptocurrency


### Bitcoin Addresses

### Implementing Keys and Addresses in Python

### Wallets

### Advanced Keys and Addresses


## Ch05 Transactions
Introduction
Transaction Lifecycle
Transaction Structure
Transaction Outputs and Inputs
Transaction Chaining and Orphan Transactions
Transaction Scripts and Script Language
Standard Transactions
## Ch06 The Bitcoin Network
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
## Ch07 The Blockchain
Introduction
Structure of a Block
Block Header
Block Identifiers: Block Header Hash and Block Height
The Genesis Block
Linking Blocks in the Blockchain
Merkle Trees
Merkle Trees and Simplified Payment Verification (SPV)
## Ch08 Mining and Consensus
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
## Ch09 Alternative Chains, Currencies, and Applications
A Taxonomy of Alternative Currencies and Chains
Meta Coin Platforms
Alt Coins
Noncurrency Alt Chains
Future of Currencies
## Ch10 Bitcoin Security
Security Principles
User Security Best Practices
Conclusion

## Appendix Transaction Script Language Operators, Constants, and Symbols
## Appendix Bitcoin Improvement Proposals
## Appendix pycoin, ku, and tx
## Appendix Available Commands with sx Tools
