
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

#### Input & Output "addresses" are really scripts
* Input **scriptSig**
```
304000202...
0467d9e39...
```

* Output **scriptPubKey**
```
OP_DUP
OP_HASH160
69e29eds...
OP_EQUALVERIFY_OP_CHECKSIG
```
> To Verify: Concatenated scripts must execute completely with no errors

#### Bitcoin scripting language ("Script")
Design goals
* Built for Bitcoin (inspired by Forth)
* Simple, compact
* Support for cryptography
* Stack-based
* Limits on time/memory
* No looping

It's not a turning compute language

#### Bitcoin script execution example
Example a script where the sender of coins simply specifies the public key of the recipient, and the recipient of the coins, to redeem them, has to specify a signature using that specified public key.

```
<sig> <pubKey> OP_DUP OP_HASH160 <pubKeyHash?> OP_EQUALVERIFY OP_CHECKSIG
```
`<sig>` data instruction: signature.  
`<pubKey>` data instruction: public key use to generate the signature.  
`OP_DUP` will duplicate the public key.  
`OP_HASH160` will hash the the value of the top (the public key), the result is `<pubKeyHash>`.  
`<pubKeyHash?>` data instruction: the public key that the sender specified, had to be used to generate the signature to redeem these coins.  
`OP_EQUALVERIFY` will compare and consume the 2 data  from the top: `<pubKeyHash?>` and `<pubKeyHash>`.  
`OP_CHECKSIG` will check and consume the 2 data  from the top:`<pubKey>` and `<sig>`, verifies that the entire transaction was successfully signed.  
There is nothing left on the stack, and if we don't have any error, the output of the script will be yes.
If we have errors, the whole transaction will be invalid.  

* There is no memory, only Stack
* data instructions are pushed directly to the stack

#### Bitcoin script instructions
256 opcodes total (15 disabled, 75 reserved)
* Arithmetic
* If/then
* Logic/data handling
* Crypto!
  * Hashes
  * Signature verification
  * Multi-signature verification `OP_CHECKMULTISIG`
    * Built-in support for joint signatures
    * Specify n public keys
    * specify t (threshold)
    * verification requires t signatures of the n pk
    * BUG ALERT: extra data value popped from the stack and ignored. High cost to remove it.

#### Proof-of-Burn
The coins have been destroyed, there's no possible way for them to be spent.

```
OP_RETURN
<arbitrary data>
```
Uses:
1. write arbitrary data into the blockchain: your name, timestamp
2. bootstrap an alternative to Bitcoin by forcing people to destroy Bitcoin in order to gain coins in the new system.

#### Idea: use the hash of redemption script
the sender specifies a very simple script
```
OP_HASH160
<hash of redemption script>
OP_EQUAL
```
the receiver
```
<signature>
<<pubkey> OP_CHECKSIG>
```
there are 2 steps: check taht redemption script had the right hash and then the redemption script will be deserialized and run as a script itself (signature check).
> pay to script hash, an alternative to the normal mode of operation, which is pay to a public key. This was not part of the initial design specification.


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
