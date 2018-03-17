# Links
## ICO
### France
http://www.coindesk.com/blockchain-in-france-a-primer-on-an-emerging-market/
* [Stratumn](https://stratumn.com/) [github](https://github.com/stratumn)
* [iEx.ec](http://iex.ec/)
* Chaintech, Frenchtech

### Analysis
http://hackingdistributed.com/2017/06/19/bancor-is-flawed/

## Incidents
### Blockchain Graveyard
[magoo](https://magoo.github.io/Blockchain-Graveyard/)
[advice](https://magoo.github.io/Blockchain-Graveyard/advice/)

### Etherum
[blog.ethereum](https://blog.ethereum.org/2016/06/17/critical-update-re-dao-vulnerability/)

## Bitcoin

## Blockchain
### The Blockchain Explained to Web Developers, Part 1: The Theory
[marmelab](http://marmelab.com/blog/2016/04/28/blockchain-for-web-developers-the-theory.html)

#### Definitions
A blockchain is a **ledger** of **facts**, replicated across several computers assembled in a peer-to-peer network. Facts can be anything from monetary transactions to content signature. Members of the network are anonymous individuals called **nodes**. All communication inside the network takes advantage of cryptography to securely identify the sender and the receiver. When a node wants to add a fact to the ledger, a consensus forms in the network to determine where this fact should appear in the ledger; this consensus is called a **block**.

#### Ordering Facts
To guarantee integrity over a P2P network, you need a way to make everyone agree on the ordering of facts. You need a **consensus system**.
Consensus algorithms for distributed systems are a very active research field. You may have heard of Paxos or Raft algorithms.
Blockchains implements the **proof-of-work** consensus, using blocks.

#### Blocks
Blocks are a smart trick to order facts in a network of non-trusted peers. The idea is simple: facts are grouped in blocks, and there is only a single chain of blocks, replicated in the entire network. Each block references the previous one. Before being added to a block, facts are pending, i.e. unconfirmed.

#### Mining
Some nodes in the chain create a new local block with pending facts. They compete to see if their local block is going to become the next block in the chain for the entire network, by rolling dice. If a node makes a double six, then it earns the ability to publish their local block, and all facts in this block become confirmed. This block is sent to all other nodes in the network. All nodes check that the block is correct, add it to their copy of the chain, and try to build a new block with new pending facts.  
Finding the random key to validate a block is very unlikely, by design. This prevents fraud, and makes the network safe (unless a malicious user owns more than half of the nodes in the network).  As a consequence, new blocks gets published to the chain at a fixed time interval. In Bitcoin, blocks are published every 10 minutes on average.

> In Bitcoin, the challenge involves a double SHA-256 hash of a string made of the pending facts, the identifier of the previous block, and a random string. A node wins if their hash contains at least n leading zeroes.
> ```
    > // a losing hash for Bitcoin
    787308540121f4afd2ff5179898934291105772495275df35f00cc5e44db42dd
    // a winning hash for Bitcoin if n=10
    00000000009f766c17c736169f79cb0c65dd6e07244e9468bc60cde9538b551e
> ```
> Number n is adjusted every once in a while to keep block duration fixed despite variations in the number of nodes. This number is called the **difficulty**.

The process of looking for blocks is called mining.

#### Money and Cryptocurrencies
* Reading data is free
* Adding facts costs a small fee
* Mining a block brings in the money of all the fees of the facts included in the block

#### Contracts
Some blockchains allow each fact to contain a mini program. Such programs are replicated together with the facts, and every node executes them when receiving the facts. In bitcoin, this can be used to make a transaction conditional: Bob will receive 100 BTC from Alice if and only if today is February 29th.
Other blockchains allow for more sophisticated contracts. In Ethereum for instance, each contract carries a mini-database, and exposes methods to modify the data. As contracts are replicated across all nodes, so are their database. Each time a user calls a method on the contract and therefore updates the underlying data, this command is replicated and replayed by the entire network. This allows for a distributed consensus on the execution of a promise.
This idea of pre-programed conditions, interfaced with the real world, and broadcasted to everyone, is called a **smart contract**.
The Blockchain revolution won’t happen unless the IoT revolution comes first.
Such applications relying on smart contracts are called Decentralized Apps, or DApps.

#### Sumsmart
How it works From a technical point of view, the blockchain is an innovation relying on three concepts: peer-to-peer networks, public-key cryptography, and distributed consensus based on the resolution of a random mathematical challenge. None of there concepts are new. It’s their combination that allows a breakthrough in computing.

Facts stored in the blockchain can’t be lost. Facts in the blockchain can be trusted. Storing data in the blockchain isn’t fast, as it requires a distributed consensus.

**example applications**: Monegraph, La zooz, Augur, Storj.io, Muse, Ripple.
* online bookmaker [Augur](https://www.augur.net/) | [github](https://github.com/AugurProject/augur.git)
* peer-to-peer storage system [storj](https://storj.io/) | [github](https://github.com/Storj/)

Many successful businesses on the Internet today are intermediaries. That’s why a technology that allows to remove intermediaries can potentially disrupt the entire Internet.

**open-source blockchain implementations**: Etherum, Hyperledger, Eris Industries (now Monax)
The maturity of these implementations varies a lot. If you have to build an application now, we’d advise:
* [Monax](https://github.com/eris-ltd) for a closed Blockchain, or to discover and play with the technology
* Ethereum for a shared Blockchain

### The Blockchain Explained to Web Developers, Part 2: In Practice
[marmelab] (http://marmelab.com/blog/2016/05/20/blockchain-for-web-developers-in-practice.html)

decentralized ad server called Zero Dollar Homepage

#### Application Concept
The blockchain shines when it replaces intermediaries. We chose to focus on Ad Platforms. Our project was to build a decentralized ad platform running on the blockchain.

#### Research, documentation and first attempts
We choose Ethereum. Use Homestead. For Doc recomendation Monax blockchain and smart contracts (they especially built a layer on top of Ethereum, and open-sourced a bunch of tools to ease the process of developing smart contracts).

`eris` is command line tool you can use to initialize any number of local blockchains you need.

#### Smart Contract Implementation
A smart contract is very similar to an API. It has a few public functions which might be called by anyone registered on the blockchain network. Unlike an API, a smart contract cannot call external web APIs (a blockchain is a closed ecosystem). A smart contract may however call other smart contracts, provided it knows their address. A contract might be in fact composed of many private functions, variables, etc.

Smart contracts are hosted in the blockchain in an Ethereum-specific binary format, executable by the **Ethereum Virtual Machine**.
Language to write contracts in Ethereum: **Solidity** looks like JS it is still very young and incomplete [doc solidity](http://solidity.readthedocs.io/en/latest/).

#### Unit Testing Contracts
The Eris guys made a tool for that, too: [sol-unit](https://github.com/smartcontractproduction/sol-unit).

#### Running a Test Blockchain
Running a blockchain and deploying our contract to it was as simple as following the Eris documentation[Monax Docs](https://monax.io/docs/tutorials/getting-started/index.html).

#### From Eris to Ethereum
At this point, we wanted to try out our contracts on a local Ethereum blockchain.

To communicate with contracts inside the Ethereum blockchain, we had to use the Web3 libraries. We learned a lot while trying to use them. We realized that eris was hiding a lot of the underlying complexity.

#### Deployment to Production
Running our application in a local environment was a challenge, but deploying it to production, in the real Ethereum network, was a battle.
[souce code github ZeroDollarHomePage](https://github.com/marmelab/ZeroDollarHomePage)

#### Debugging
The Ethereum developer experience is very bad.
The tools available to track Ethereum contracts and transactions are:
etherscan.io: displays data about contracts, transactions, blocks
etherchain.org: The blocks and ether information
You can also get aggregated stats about the network and the nodes

#### documentation
Here are a few resources worth reading if you want to start developing smart contracts yourself:
* A 101 Noob Intro to Programming Smart Contracts on Ethereum
* The Ethereum Guide
* The Ethereum Github wiki


### The Blockchain Explained to Web Developers, Part 3: The Truth
[marmelab](http://marmelab.com/blog/2016/06/14/blockchain-for-web-developers-the-truth.html)

### Blockchain technology explained by Sal Khan
[blockchainpie: blockchain-tech-explained](http://blockchainpie.com/blockchain-technology-explained-by-salkhan/)
