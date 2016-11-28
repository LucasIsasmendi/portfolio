# Bitcoin and Cryptocurrency Technologies
[coursera: bitcoin](https://www.coursera.org/learn/cryptocurrency/home/welcome)

## Week 1 - Introduction to Crypto and Cryptocurrencies

### Cryptographic Hash Functions
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
  <img src="/images/SHA-256 hash function.jpg" width="700">
  Theorem: If c is collision-free, then SHA-256 is collision-free.
</p>



## Week 2

## Week 3

## Week 4

## Week 6

## Week 7

## Week 8

## Week 9

## Week 10

## Week 11
