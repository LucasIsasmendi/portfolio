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




## Week 2

## Week 3

## Week 4

## Week 6

## Week 7

## Week 8

## Week 9

## Week 10

## Week 11
