# Mastering Blockchain

## Ch01 Blockchain 101

Blockchain technology is currently at the peak of inflated expectations (as of July 2016) and is expected to be ready for mainstream adoption in 5 to 10 years.

Various benefits of this technology are being envisaged such as decentralized trust, cost savings, transparency, and efficiency. However, there are various challenges too that are an area of active research such as scalability and privacy.

In 2008 a groundbreaking paper Bitcoin: A Peer-to-Peer Electronic Cash System was written on the topic of peer-to-peer electronic cash under the pseudonym Satoshi Nakamoto and introduced the term chain of blocks. This term over the years has now evolved into the word blockchain.

### Distributed systems

Blockchain at its core is a distributed system. More precisely it is a decentralized distributed system.
A node can be defined as an individual player in a distributed system.
A node that can exhibit arbitrary(unexpected or malicious) behavior is also known as a _Byzantine_ node.
The main challenge in distributed system design is coordination between nodes and fault tolerance.

### CAP theorem (Brewer's theorem - 1998)

The theorem states that any distributed system cannot have Consistency, Availability, and Partition tolerance simultaneously:

- **Consistency** is a property that ensures that all nodes in a distributed system have a single latest copy of data
- **Availability** means that the system is up, accessible for use, and is accepting incoming requests and responding with data without any failures as and when required
- **Partition tolerance** ensures that if a group of nodes fails the distributed system still continues to operate correctly

In order to achieve fault tolerance, replication is used. This is a common and widely used method to achieve fault tolerance. Consistency is achieved using consensus algorithms to ensure that all nodes have the same copy of data. This is also called **state machine replication**. Blockchain is basically a method to achieve state machine replication.

### Byzantine Generals problem

- In September 1962, Paul Baran introduced the idea of cryptographic signatures with his paper On _distributed communications networks_.
- Then in 1982 a thought experiment was proposed by Lamport et al. whereby a group of army generals who are leading different parts of the Byzantine army are planning to attack or retreat from a city. Traitors can be considered Byzantine (malicious) nodes.
- In 1999 by Castro and Liskov who presented the Practical Byzantine Fault Tolerance (PBFT) algorithm solves the experiment: The only way of communication between them is a messenger and they need to agree to attack at the same time in order to win. The issue is that one or more generals can be traitors and can communicate a misleading message.
- Later on in 2009, the first practical implementation was made with the invention of bitcoin where the _Proof of Work (PoW)_ algorithm was developed as a mechanism to achieve consensus.

### Consensus

Consensus is a process of agreement between distrusting nodes on a final state of data.
The concept of achieving consensus between multiple nodes is known as distributed consensus.

#### Consensus mechanisms

Steps that are taken by all, or most, nodes in order to agree on a proposed state or value.
There are various requirements which must be met in order to provide the desired results in a consensus mechanism.

- **Agreement**: All honest nodes decide on the same value.
- **Termination**: All honest nodes terminate execution of the consensus process and eventually reach a decision.
- **Validity**: The value agreed upon by all honest nodes must be the same as the initial value proposed by at least one honest node.
- **Fault tolerant**: The consensus algorithm should be able to run in the presence of faulty or malicious nodes (Byzantine nodes).
- **Integrity**: This is a requirement where by no node makes the decision more than once. The nodes make decisions only once in a single consensus cycle.

##### Types of consensus mechanism

- **Byzantine fault tolerance-based**: With no compute intensive operations such as partial hash inversion, this method relies on a simple scheme of nodes that are publishing signed messages. Eventually, when a certain number of messages are received, then an agreement is reached.
- **Leader-based consensus mechanisms**: This type of mechanism requires nodes to compete for the leader-election lottery and the node that wins it proposes a final value.

**Paxox**: protocol introduced by Leslie Lamport in 1989: nodes are assigned various roles such as Proposer, Acceptor, and Learner. Nodes or processes are named replicas and consensus is achieved in the presence of faulty nodes by agreement among a majority of nodes.

**RAFT**:  which works by assigning any of three states, that is, Follower, Candidate, or Leader, to the nodes. A Leader is elected after a candidate node receives enough votes and all changes now have to go through the Leader, who commits the proposed changes once replication on the majority of follower nodes is completed.

>**ToDo**: Research about theory of consensus mechanisms from a distributed system point of view.

### The history of blockchain

Blockchain was introduced with the invention of bitcoin in 2008 and then with its practical implementation in 2009.
The concept of electronic cash or digital currency is not new. Since the 1980s, e-cash protocols have existed that are based on a model proposed by David Chaum.

The various ideas that helped with the invention of bitcoin and blockchain:

- e-cash schemes
- state machine replication
- consensus mechanism
- hashcash computational puzzles
- P2P networks

#### Electronic cash

ideas from different electronic cash schemes also paved the way for the invention of cryptocurrencies, specifically bitcoin.

##### The concept of electronic cash

Fundamental issues that need to be addressed in e-cash systems are accountability and anonymity. David Chaum addressed both of these issues in his seminal paper in 1984 by introducing two cryptographic operations, namely blind signatures and secret sharing (avoid double spending).

After this other protocols emerged such as **Chaum, Fiat, and Naor (CFN)**, e-cash schemes that introduced anonymity and double spending detection. Brand's e-cash is another system that improved on CFN, made it more efficient.

A different but relevant concept called **hashcash** was introduced by Adam Back in 1997 as a PoW system to control e-mail spam. Hashcash is popularized by its use in the bitcoin mining process.

In 1998 **b-money** was introduced by Wei Dai and proposed the idea of creating money via solving computational puzzles such as hashcash. It's based on a peer-to-peer network where each node maintains its own list of transactions.

In 2009 the first practical implementation of a cryptocurrency named bitcoin was introduced; for the very first time it solved the problem of distributed consensus in a trustless network. It uses public key cryptography with hashcash as PoW to provide a secure, controlled, and decentralized method of minting digital currency. The key innovation is the idea of an ordered list of blocks composed of transactions and cryptographically secured by the PoW mechanism.

### Introduction to blockchain

**Blockchain** at its core is a peer-to-peer distributed ledger that is cryptographically secure, append-only, immutable (extremely hard to change), and updateable only via consensus or agreement among peers.

<p align="center">
  <img src="/images/books/mastering-blockchain/ch1-01-network view of a blockchain.jpeg" width="700">
</p>
> Blockchain can be thought of as a layer of a distributed peer-to-peer network running on top of the Internet

From a business point of view a blockchain can be defined as a platform whereby peers can exchange values using transactions without the need for a central trusted arbitrator.

### Generic elements of a blockchain

- Addresses: to denote senders and recipients.
- Transaction: transafer of value.
- Block: multiple transactions and some other elements such as the previous block hash, timestamp, and nonce.
- Peer-to-peer network
- Scripting or programming language
- Virtual machine
- State machine
- Nodes
- Smart contracts

### Features of a blockchain

- Distributed consensus
- Transaction verification
- Platforms for smart contracts
- Transferring value between peers
- Generating cryptocurrency
- Smart property
- Provider of security
- Immutability
- Uniqueness
- Smart contracts

### Applications of blockchain technology

Almost all industries have already realized the potential and promise of blockchain and have already embarked, or soon will embark, on the journey to benefit from the blockchain technology.

#### How blockchains accumulate blocks

1. A node starts a transaction by signing it with its private key.
2. The transaction is propagated (flooded) by using much desirable Gossip protocol to peers, which validates the transaction based on pre-set criteria. Usually, more than one node is required to validate the transactions.
3. Once the transaction is validated, it is included in a block, which is then propagated on to the network. At this point, the transaction is considered confirmed.
4. The newly created block now becomes part of the ledger and the next block links itself cryptographically back to this block. This link is a hash pointer. At this stage, the transaction gets its second confirmation and the block gets its first.
5. Transactions are then reconfirmed every time a new block is created. Usually, six confirmations in the bitcoin network are required to consider the transaction final.

Steps 4 and 5 can be considered non-compulsory as the transaction itself is finalized in step 3; however, block confirmation and further transaction reconfirmations, if required, are then carried out in steps 4 and 5.

### Tiers of blockchain technology

originally described by Melanie Swan in her book Blockchain, Blueprint for a New Economy

- **Blockchain 1.0**: invention of bitcoin and is basically used for cryptocurrencies.
- **Blockchain 2.0**: used by financial services and contracts are introduced in this generation.
- **Blockchain 3.0**: used to implement applications beyond the financial services industry and are used in more general-purpose industries such as government, health, media, the arts, and justice.
- **Generation X (Blockchain X)**: one day we will have a public blockchain service available that anyone can use just like the Google search engine. It will provide services in all realms of society.

### Types of blockchain

- Public blockchains (permission-less ledgers)
- Private blockchains
- Semi-private blockchains
- Sidechains (pegged sidechains)
- Permissioned ledger (agreement protocol): participants of the network are known and already trusted
- Distributed ledger: records are stored contiguously instead of sorted into blocks. This concept is used in Ripple.
- Shared ledger
- Fully private and proprietary blockchains
- Tokenized blockchains: generate cryptocurrency as a result of a consensus process via mining or via initial distribution.
- Tokenless blockchains

### Consensus in blockchain
distributed computing concept that has been used in blockchain in order to provide a means of agreeing to a single version of truth by all peers on the blockchain network.
Two categories of consensus mechanism exist:

1. Proof-based, leader-based, or the Nakamoto consensus whereby a leader is elected and proposes a final value
2. Byzantine fault tolerance-based, which is a more traditional approach based on rounds of votes

**Important consensus algorithms**

- Proof of Work: enough computational resources have been spent before proposing a value for acceptance by the network. Currently, this is the only algorithm that has proven astonishingly successful against Sybil attacks.
- Proof of Stake (PoS): This idea was first introduced by Peercoin and is going to be used in the Ethereum blockchain.
- Delegated Proof of Stake (DPOS): is an innovation over standard PoS, used in the bitshares blockchain.
- Proof of Elapsed Time: Introduced by Intel, it uses Trusted Execution Environment (TEE) to provide randomness and safety in the leader election process via a guaranteed wait time (by processor). Used in Hyperledger
- Deposit-based consensus: put in a security deposit before they can propose a block.
- Proof of importance: not only relies on how much stake a user has in the system but it also monitors the usage and movement of tokens, used in Nemcoin.
- Federated consensus or federated Byzantine consensus: Used in the stellar consensus protocol, nodes in this protocol keep a group of publicly trusted peers and propagates only those transactions that have been validated by the majority of trusted nodes.
- Reputation-based mechanisms
- Practical Byzantine Fault Tolerance (PBFT): achieves state machine replication, which provides tolerance against Byzantine nodes.
- Various other protocols, including but are not limited to PBFT, PAXOS, RAFT, and Federated Byzantine Agreement (FBA), are also being used or have been proposed for use in many different implementations of distributed systems and blockchains.

### CAP theorem and blockchain

**Consistency (C)** on the blockchain is not achieved simultaneously with **Partition tolerance (P)** and **Availability (A)**, but it is achieved over time. This is called **eventual consistency**, where consistency is achieved as a result of validation from multiple nodes over time.

### Benefits and limitations of blockchain

top benefits

- Decentralization
- Transparency and trust
- Immutability
- High availability
- Highly secure
- Simplification of current paradigms
- Faster dealings
- Cost saving

#### Challenges and limitations of blockchain technology

- Scalability
- Adaptability
- Regulation
- Relatively immature technology
- Privacy
