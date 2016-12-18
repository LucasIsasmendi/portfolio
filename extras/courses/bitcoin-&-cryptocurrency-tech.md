# Bitcoin and Cryptocurrency Technologies
[coursera: bitcoin](https://www.coursera.org/learn/cryptocurrency/home/welcome)

## Week 1 - Introduction to Crypto and Cryptocurrencies

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

## Week 2 - How Bitcoin Achieves Decentralization

### 2.1 Centralization vs. Decentralization


### 2.2 Distributed Consensus


### 2.3 Consensus without Identity: the Block Chain


### 2.4 Incentives and Prrof of Work


### 2.5 Putting it All Together


## Week 3

## Week 4

## Week 6

## Week 7

## Week 8

## Week 9

## Week 10

## Week 11
