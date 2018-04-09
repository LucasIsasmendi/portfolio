# Delegated Proof-of-Stake

[bitshares](https://bitshares.org/technology/delegated-proof-of-stake-consensus/)

is the fastest, most efficient, most decentralized, and most flexible consensus model available.
DPOS leverages the power of stakeholder approval voting to resolve consensus issues in a fair and democratic way.
All network parameters, from fee schedules to block intervals and transaction sizes, can be tuned via elected delegates. Deterministic selection of block producers allows transactions to be confirmed in an average of just 1 second. Perhaps most importantly, the consensus protocol is designed to protect all participants against unwanted regulatory interference.

BitShares is first and foremost a globally distributed database that is used as a ledger to track ownership of digital assets. All updates to the ledger must be validated and applied in the proper order to allow the database to remain consistent and universally agreed upon.

The questions that must be answered by any consensus process include, but are not limited to:
1. Who should produce the next block of updates to apply to the database?
2. When should the next block be produced?
3. What transactions should be included in the block?
4. How are changes to the protocol applied?
5. How should competing transaction histories be resolved?

* Block Production by Elected Witnesses
* Parameter Changes by Elected Delegates
* Changing the Rules (aka Hard Forks): all changes must be triggered by active stakeholder approval.
* Double Spend Attack: the probability of a communication breakdown enabling a double spend attack is very low.
* Transactions as Proof of Stake
* Blockchain Reorganizations: The software always selects the blockchain with the highest witness participation rate.
* Maximally Decentralized
