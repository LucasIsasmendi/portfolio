# Bitcoin and Cryptocurrency Technologies
[coursera: bitcoin](https://www.coursera.org/learn/cryptocurrency/home/welcome)

## Index
[W1-Introduction to Crypto and Cryptocurrencies](#w1-introduction-to-crypto-and-cryptocurrencies)  
* [1.1 Cryptographic Hash Functions](#11-cryptographic-hash-functions)  
* [1.2 Hash Pointers and Data Structures](#12-hash-pointers-and-data-structures)  
* [1.3 Digital Signatures](##13-digital-signatures)  
* [1.4 Public Keys as Identities](#14-public-keys-as-identities)
* [1.5 A Simple Cryptocurrency](#15-a-simple-cryptocurrency)


[W2-How Bitcoin Achieves Decentralization](#w2-how-bitcoin-achieves-decentralization)  
* [2.1 Centralization vs. Decentralization](#21-centralization-vs.-decentralization)
* [2.2 Distributed Consensus](#22-distributed-consensus)
* [2.3 Consensus without Identity: the Block Chain](#23-consensus-without-identity-the-block-chain)
* [2.4 Incentives and Prof of Work](#24-incentives-and-prof-of-work)
* [2.5 Putting it All Together](#25-putting-it-all-together)

[W3-Mechanics of Bitcoin](#w3-mechanics-of-bitcoin)
* [3.1 Bitcoin Transactions](#31-bitcoin-transactions)
* [3.2 Bitcoin Scripts](#32-bitcoin-scripts)
* [3.3 Applications of Bitcoins Scripts](#[33-applications-of-bitcoins-scripts)
* [3.4 Bitcoin Blocks](#34-bitcoin-blocks)
* [3.5 The Bitcoin Network](#35-the-bitcoin-network)
* [3.6 Limitations and Improvements](#36-limitations-and-improvements)



## W1-Introduction to Crypto and Cryptocurrencies

### 1.1 Cryptographic Hash Functions
#### Hash function:
*  takes any string as input
* fixed-size output (we'll use 256 bits)
* efficiently computable

#### Security properties:

#### collision-free

* Nobody can can find x and y such that `x!=y` and `H(x)=H(y)` {collision, H:Hash}
* collisions do exist, but can anyone find them?
* How to find a collision: `2^130` tries with `99.8%` success (too long to matter)
* We choose to believe that H() are collision-free
* **Application: Hash as message digest**  
  if we know `H(x)=H(y)`, it's safe to assume that `x=y`. To recognize a file that we saw before, just remember its hash. Useful because the hash is small.

#### hiding

* We want something like this: Given `H(x)`, it is infeasible to find `x`.
* **Hiding property**:
  if `r` is chosen from a probability distribution that has *high min-entropy*, then given `H(r | x)`, it is infeasible to find `x`.  
  *High min-entropy* means that the distribution is "very spread out", so that no particular value is chosen with more than negligible probability.

* **Application: Commitment**
  Want to "seal a value in an envelope", and "open the envelope" later.
  Commit to a value, reveal it later.  

  **Commitment API: process**  
  `(com, key)` := `commit(msg)`  
  `match` := `verify(com, key, msg)`  
  To seal `msg` in envelope: (com, key) := `commit(msg)` -- then publish `com`
  To open envelope: publish `key`, `msg` anyone can use `verify()` to check validity  

  Security properties:
  - Hiding: Given `com`, infeasible to find `msg`.
  - Binding: Infeasible to find `msg!=msg'` such that `verify(commit(msg), msg') == true`

  **Commitment API: process**  
  `commit(msg)` := `( H(key|msg), key )` where key is a random 256-bit value  
  `verify(com, key, msg)` := `( H(key|msg) == com)`

  Security properties:
  - Hiding: Given `H(key|msg)`, infeasible to find `msg`.
  - Binding: Infeasible to find `msg!=msg'` such that `H(key|msg) == H(key|msg')`
  >pipe "|" is concatenate

#### puzzle-friendly
For every possible output value `y`, if `k` is chosen from a distribution with high min-entropy, then it is infeasible to find `x` such that `H(k | x) = y`.

* **Application: Search puzzle**  
  Given a "puzzle ID" `id` (from high min-entropy distrib.), and a target set `Y`: Try to find a "solution" `x` such that `H(id | x) E Y`.

  Puzzle-friendly property implies that no solving strategy is much better than trying random values of x.

Hash Function that bitcoin uses and we will use is **SHA-256 hash function**

<p align="center">
  <img src="/images/bitcoin-crypt/w1-1.1-SHA-256 hash function.jpg" width="700">
</p>

> Theorem: If c is collision-free, then SHA-256 is collision-free.

### 1.2 Hash Pointers and Data Structures

hash pointer is:
  * pointer to where some info is stored, and
  * (cryptographic) hash of the info

if we have hash pointer, we can
  * ask to get the info back, and
  * verify that it hasn't changed

<p align="center">
  <img src="/images/bitcoin-crypt/w1-1.2-hash-pointers-and-dt.jpg" width="300">
</p>

> key idea: build data structures with hash pointers

For example here is a linked list that we built with hash pointers. And this is a data structure that we're gonna call a **block chain**.

**detecting tampering**

<p align="center">
  <img src="/images/bitcoin-crypt/w1-1.2-detecting-tampering.jpg" width="450">
</p>

> use case: tamper-evident log

And so what this means is that just
by remembering this hash pointer `H()`, we've essentially remembered a kind of hash, a tamper evident hash of the entire list all the way back to the beginning (the genesis block).

**binary tree with hash pointers = "Merkle tree"**

<p align="center">
  <img src="/images/bitcoin-crypt/w1-1.2-Merkle tree.jpg" width="450">
</p>

Another useful data structure that we can build using hash pointers is a binary tree.

**providing membership in a Merkle tree**

<p>
  <img src="/images/bitcoin-crypt/w1-1.2-Merkle tree membership.jpg">
</p>

Unlike the block chain that we built before, that if someone wants to prove to us that a particular data block is a member of this Merkle tree. All they need to show us is this amount of data. So that takes about log n items that we need to be shown, and it takes about log n time for us to verify it. And so at the very large number of data blocks in the Merkle tree, we can still verify proven membership in a relatively short time.

**Advantages of Merkle trees**

* The tree holds many items but we just need to remember the one
root hash which is only 256 bits.
* We can verify membership in a Merkle tree in logarithmic time and logarithmic space O(log n).
* Varian: sorted Merkle tree can verify non-membership in O(log n) (show items before, after the missing one)


**More generally...**
can use hash pointers in any pointer-based data structure that has no cycles.

### 1.3 Digital Signatures

**What we want from signatures**

Only you can sign, but anyone can verify.

Signature is tied to a particular document, can't be cut-and-pasted to another doc.

#### API for digital signatures

`(sk, pk)` := `generateKeys(keysize)`   
 > sk: secret signing key  
 >pk: public verification key

`sig` := `sign(sk, message)`   

`isValid` := `verify(pk, message, sig)`   

* `(sk, pk)` and `sig` can be randomized algorithms

#### Requirements for signatures

**"valid signatures verify"**  
verify(pk, message, sign(sk, message)) == true

**"can't forge signatures"**  
adversary who:
- knows pk
- gets to see signatures on message of his choice  

can't produce a verifiable signature on another message

GAME: challenger and attacker: generate (sk,pk), the attacker only knows the pk, the challenger can sign messages. Attacker send messages and receive signed messages. Then will try to force a signature in a message that was not send previously.

<p>
  <img src="/images/bitcoin-crypt/w1-1.3-Game.jpg">
</p>
> the attacker's probability of winning this game is negligible, no matter what algorithm the attacker is using.

#### Practical Stuff

* algorithms are randomized,
  need good source of randomness

* limit on message size,
  fix: use Hash(message) rather than message

* fun trick: sign hash pointer,
  signature "covers" the whole structure: if you were to sign
the hash pointer that was at the end of a block chain, the result
is that you would effectively be digitally signing the entire
contents of that block chain.

#### Nuts and Bolts
Bitcoins uses **ECDSA**  standard Elliptic Curve Digital Signature Algorithm (US government standard)

Relies on hairy math (will skip the details here)

**Good randomness is essential**, foul this up in generateKeys() or sign()?, probably leaked your private key *GAME OVER*


### 1.4 Public Keys as Identities

**Useful trick: public key == an identity**

if you see sig such that `verify(pk, msg, sig) == true`, think of it as `pk says, "[msg]"`.

to "speak for" pk, you must know matching secret key sk.

#### How to make a new identity

create a new, random key-par (sk, pk).  
`pk` is the public "name" you can use [usually better to use Hash(pk)]  
`sk` lets you "speak for" the identity

You control the identity, because only you know sk if pk "looks random", nobody needs to know who you are

#### Decentralized identity Management

Anybody can make a new identity at any time make as many as you want!

no central point of coordination

These identities are called "addresses" in Bitcoin.

#### Privacy (how private is it?)
* Addresses not directly connected to real-world identity.
* But observer can link together an address's activity over time, make inferences.
* Later: a whole lecture on privacy in Bitcoin...

### 1.5 A Simple Cryptocurrency

#### Goofy coins

* Goofy can create new coins
* A coin's owner can spend it.
* The recipient can pass on the coin again.

#### Problem
* double-spending attack: the main design challenge in digital currency  

#### Solution: ScroogeCoin
* Scrooge publishes a history of all transactions (a block chain, signed by Scrooge)
> optimization: put multiple transactions in the same block

* CreateCoins transaction creates new coins
* PayCoins transaction consumes (and destroys) some coins, and creates new coins of the same total value. Valid if:
  - consumed coins valid,
  - not already consumed,
  - total value out = total value in, and
  - signed by owners of all consumed coins

**Immutable coins**  
Coins can't be transferred, subdivided, or combined.

But: you can get the same effect by using transactions to subdivide: create new trans, consume your coin, pay out two new coins to yourself

**Crucial question:**  
Can we descroogify the currency, and operate without any central, trusted party?

### Programming Assignment
assignment1starterCode.zip

## W2-How Bitcoin Achieves Decentralization

### 2.1 Centralization vs. Decentralization

The way in which Bitcoin achieves decentralization is not purely technical but it's a combination of technical and clever incentive engineering.

Centralizarion vs. decentralization are competing paradigms that underlie many digital technologies.

#### Decentralization is not all-or-nothing
E-mail: decentralized protocol, but dominated by centralized webmail services.

#### Aspect of decentralization in Bitcoin

1. Who maintains the ledger?
2. Who has authority over which transactions are valid?
3. Who creates new bitcoins?
4. Who determines how the rules of the system change?
5. How do bitcoins acquire exchange value?

Beyond the protocol: exchanges, wallet software, service providers..

#### Aspects of decentralization in Bitcoin
* Peer-to-peer network: open to anyone, low barrier to entry
* Mining: open to anyone, but inevitable concentration of power often seen as undesirable
* Updates to software: core developers trusted by community, have great power

### 2.2 Distributed Consensus
#### Bitcoin's key challenge
Key technical challenge of decentralized e-cash: **distributed consensus**  
or: how to decentralize ScroogeCoin

#### Why consensus protocols?
Traditional motivation: reliability in distributed systems

**Distributed key-value store** enables various applications: DNS, public key directory, stock trades... => **Good targets for Altcoins!**

**Defining distributed consensus** the protocol terminates and all correct nodes decide on the same value. This value must have been proposed by some correct node.

#### bitcoin is a peer-to-peer system
When Alice wants to pay Bob: she broadcasts the transaction to all Bitcoin nodes.  
{ signed by Alice, Pay to pk_Bob: H( ) } Bob listening is not necessary.

#### How consensus could work in Bitcoin
At any given time:
* All nodes have a sequence of *blocks of transactions* they've reached consensus on
* Each node has a set of outstanding transactions it's heard about

#### Why consensus is hard
Nodes may crash
Nodes may be malicious
Network is imperfect
* Not all pairs of nodes connected
* Faults in network
* Latency => No notion of global time
* Many impossibility results
  - Byzantine generals problem
  - Fischer-Lynch-Paterson (deterministic nodes): consensus impossible with a single faulty node

#### Some well-known protocols
Example: Paxos.  [wikipedia](https://en.wikipedia.org/wiki/Paxos_(computer_science) is a family of protocols for solving consensus in a network of unreliable processors.

Never produces inconsistent result, but can (rarely) get stuck

**Understanding impossibility results** These results say more about the model than about the problem. The models were developed to study systems like distributed databases.

#### Bitcoin consensus: theory & practice
Bitcoin consensus works better in practice than in theory.  
Theory is still catching up  
BUT theory is important, can help predict unforeseen attacks

#### Some things Bitcoin does differently
**Introduces incentives**
* Possible only because it's  currency!

**Embraces randomness**
* Does away with the notion of a specific end-point
* Consensus happens over long time scales - about 1 hour

### 2.3 Consensus without Identity: the Block Chain
#### Why identity?
Pragmatic: some protocols need node IDs  
Security: assume less than 50% malicious

**Why don't Bitcoin nodes have identities?**  
Identity is hard in a P2P system - Sybil attack (copies of nodes that a malicious attacker can create to look like different participants, but controlled by the same adversary)  

Pseudonymity is a goal of Bitcoin: your transactions can be tracked, but you are not force to put your identity to get into bitcoin

#### Weaker assumption: select random node
Analogy: lottery or raffle

When tracking & verifying identities is hard, we give people tokens, tickets, etc.

Now we can pick a random ID & select that node (the adversary is not able to multiply his power that way)

**Key idea: implicit consensus**  
In each round, random node is picked  
This node proposes the next block in the chain  
Other nodes implicitly accept/reject this block
* by either extending it
* or ignoring it and extending chain from earlier block

Every block contains hash of the block it extends

#### Consensus algorithm (simplified)
1.  New transactions are broadcast to all nodes
2.  Each node collects new transaction into a block
3.  In each round a **random** node gets to broadcast its block
4.  Other nodes accept the block only if all transactions in it are valid (unspent, valid signatures)
5.  Nodes express their acceptance of the block by including its hash in the next block they create

**What can a malicious node do?**
* Try to steal Bitcoins belonging to another user: cannot forge their signatures, so as long as the underlying crypto is solid, is not able to simply steal Bitcoins.
* Not include user transactions in the next block (denying service): the user should wait an extra round to get included into blockchain by an honest node.
* Double spending attack: decreases exponentially with number of confirmations (most common heuristic 6).

**Recap**

Protection against invalid transactions is cryptographic, but enforced by consensus.

Protection against double-spendin is purely by consensus.

You're never 100% sure a transaction is in consensus branch. Guarantee is probabilistic.

### 2.4 Incentives and Prof of Work

**Assumption of honesty is problematic**  
Can we give nodes **incentives** for behaving honestly?
`[From]:` Can we penalize the node that created the double spending attack?
`[To]:` Can we reward nodes that created honest blocks? `=>` with bitcoins (everything so far is just distributed consensus protocol. But now we utilize the fact that the currency has value)

#### Incentive 1: block reward
Creator of block gets to:
* include special coin-creation transaction in the block
* choose recipient address of this transaction

Value is fixed: currently 25 BTC, halves every 4 years.
Block creator gets to "collect" the reward only if the block ends up on long-term consensus branch!

**There's a finite supply of bitcoins**

Total Supply: 21 million. block reward is how new bitcoins are created. Runs out in 2140. No new bitcoins unless rules change.

#### Incentive 2: transaction fees
Creator of transaction can choose to make output value less than input value.  
Remainder is a transaction fee and goes to block creator.  
Purely voluntary, like a tip.

#### Remaining Problems
1. How to pick a random node?
2. How to avoid a free-for-all due to rewards?
3. How to prevent Sybil attacks?

#### Proof of Work
To approximate selecting a random node: select nodes in proportion to a resource that no one can monopolize (we hope)
* In proportion to computing power: proof-of-work
* In proportion to ownership: proof-of-stake

#### Hash puzzles
To create block, find nonce s.t. `H(nonce|prev_hash|tx|..|tx) is very small`

<img src="/images/bitcoin-crypt/w2-2.4-hash-puzless.jpg">

#### PoW property 1: difficult to compute
As of Aug 2014: about 10^20 hashes/block.  
Only some nodes bother to compete - miners

#### PoW property 2: parameterizable cost
Nodes automatically re-calculate the target every two weeks.  
Goal: average time between blocks = 10 minutes

Prob (Alice wins next block) = fraction of global hash power she controls

##### Key security assumption
Attacks infeasible if majority of miners weighted by hash power follow the protocol (competition for next block, win the prize)

**Solving hash puzzles is probabilistic**, nobody can predict
which nonce is going to result in solving the hash puzzle (Bernoulli trials can be well approximated by a continuous probability process called a poisson process = exponential distribution). The network adjusts the difficulty, so the time is always close to 10 minutes.

For individual miner: mean time to find a block = 10 minutes / fraction of hash power

#### PoW property 3: trivial to verify
Nonce must be published as part of block  
Other miners simply verify that `H(nonce|prev_hash|tx|..|tx) < target`
We don't need a central autority.

### 2.5 Putting it All Together

#### Mining economics
If mining reward (block reward + Tx fees) > hardware + electricity cost -> Profit

Complications:
* fixed vs. variable costs
* reward depends on global hash rate

#### Recap
* Identities
* Transactions
* P2P Network
* Block chain & consensus
* Hash puzzles & mining

**Bitcoin has 3 types of consensus**
* Value
* State
* Rules

#### Bitcoin is Bootstrapped

<img src="/images/bitcoin-crypt/w2-2.5-bitcoin-bootstrapped.jpg">

> these three characteristics we're required by the Bitcoin system in an interdependent manner with each other

#### What can a "51% attacker" do? (consensus failed)
Steal coins from an existing address? No, Unless you subvert the cryptography.

Suppress some transactions?
* From the block chain. Yes
* From the P2P network. No

Change the block reward? NO, The attacker doesn't control the copies of bitcoins software that we are running

Destroy confidence in Bitcoin? Yes

#### Remaining questions
How do we get from consensus to currency?  
What else can we do with consensus?

## W3-Mechanics of Bitcoin

#### Recap: Bitcoin consensus
Bitcoin consensus gives us:
* Append-only ledger
* Decentralized consensus
* Miners to validate transactions

assumming a currency exists to motivate miners!

### 3.1 Bitcoin Transactions

#### An account-based ledger (not Bitcoin)
<img src="/images/bitcoin-crypt/w3-3.1-not bitcoin-account based ledger.jpg">

#### A tranasaction-based ledger (Bitcoin)
<img src="/images/bitcoin-crypt/w3-3.1-bitcoin-transaction based ledger.jpg">
>pointers to previous transactions are in the blockchain itself

#### Merging Value
<img src="/images/bitcoin-crypt/w3-3.1-mergin value.jpg">

#### Joint payments
<img src="/images/bitcoin-crypt/w3-3.1-Joint payments.jpg">
> two signatures


#### A Bitcoin Transaction
<img src="/images/bitcoin-crypt/w3-3.1-a bitcoin transaction.jpg">

### 3.2 Bitcoin Scripts


### 3.3 Applications of Bitcoins Scripts

### 3.4 Bitcoin Blocks

### 3.5 The Bitcoin Network

### 3.6 Limitations and Improvements

## Week 4


## Week 6


## Week 7


## Week 8


## Week 9


## Week 10


## Week 11
