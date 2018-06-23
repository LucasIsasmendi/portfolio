# Token Engineering Series - Oceanprotocol

## [Part I - Can Blockchains Go Rogue?](https://blog.oceanprotocol.com/can-blockchains-go-rogue-5134300ce790)

*AI Whack-A-Mole, Incentive Machines, and Life.*  
*Trent McConaghy - oceanprotocol*

“Show me the incentive and I will show you the outcome.”

"Blockchain Superpower: Get people to do stuff by rewarding with tokens"

## [Part II - Towards a Practice of Token Engineering](https://blog.oceanprotocol.com/towards-a-practice-of-token-engineering-b02feeeff7ca)

*Methodology, Patterns & Tools.*
*Trent McConaghy - oceanprotocol*

### Token design and relation to other fields

1. Game Theory and Mechanism Design
  Game Theory is a scientific field that analyzes incentives, from an economic standpoint. It has a counterpart in economics for designing (synthesizing) incentivized systems, called Mechanism Design (reverse game theory).

2. Other Related Fields
 everything from electrical engineering to complex systems, from economics to AI (cybernetics)

3. Engineering and Token Design
  Related token labels

  - Economics (analyzing price movement, valuations):
    - Mechanism Design
    - Tokenomics
    - Crypto-economics
    - Financial cryptography
  - Engineering
    - Token engineering
    - Incentive engineering

Design of tokenized Ecosystems:
Analysis (Game Theory) <-> Synthesis (Mechanism Design) -> Optimization Design

is like Design of Evolutionary Algorithms

### Token Design as Optimization Design

- Goals: in the form of objectives (things to maximize or minimize) and constraints (things that must be met).
  Block reward function -> Maximize hash rate.
- Measurement & test: success against the goals
  Proof -> Proof of Work
- System agents: do what it takes to get more token rewards
  Miners and token holders in a network
- System clock:  time dimension by which progress is made
  Block reward interval
- Incentives & Disincentives: In designing the system, we design what rewards or punishments to give, and how to give them.
  You can't control humans, just reward (give tokens) and punish (slash stake)

### From Optimization Methodology to Token Methodology

- Optimization Methodology

  - Formulate the problem: in objectives, constraints, and design space
  - Try an existing solver: if it works well, it's wonky
  - New solver?

- Token Methodology

  - Formulate the Problem
    write down the objectives and constraints for your tokenized ecosystem. This means asking: who are my potential stakeholders, and what do each of them want? What are attack vectors? Then translating those into objectives and constraints that you can measure.
  - Try An Existing Pattern
  - New Pattern?:
    use existing building blocks where possible, from TCRs (Token-Curated Registries) to arbitration.

### Token Design Patterns : A Starting Point

**ALL OF IT** (check [My Notes on Design Patterns](./engineering/Design-Patterns/readme.md))

- Curation
- Identity
- Reputation
- Governance
- Third-party arbitration
- Proofs of human or compute work

Other ways to frame or group building blocks include:

- How tokens are distributed
- Ethereum token standards
- How tokens are valued
- How keepers are grouped
- How the compute stack is organized
- Level-1, level-2, level-N infrastructure
- Cryptoeconomic primitives

### Tools We Need: Simulators, CAD Tools

- Proper simulators of tokenized ecosystems
  - [agent-based modeling](https://www.santafe.edu/engage/learn/courses/introduction-agent-based-modeling)
- CAD tools that wrap the simulators to verify designs further

people doing token design get their friends to dream up new attacks, then they update the constraints list then the design accordingly.

Simulation will never be perfect. So, we should ensure that the system itself is evolvable, towards the intent of the community. The tools for this are governance, staking, and more. Governance may be as simple as hard forks, for example to change the objective function or add constraints. Staking helps convert zero sum to positive sum for the community of token holders.

#### Extrinsic vs. Intrinsic Motivation

> “ Extrinsic motivation is encouragement from an outside force; behavior is performed based on the expectance of an outside reward [to convince] someone into doing something that they would not do on their own.
Intrinsic means innate or within; hence intrinsic motivation is the stimulation or drive stemming from within oneself. Intrinsic motivation is often associated with intrinsic rewards because the natural rewards of a task are the motivating forces that encourage an individual in the first place.”

- Extrinsic rewards do not produce permanent changes
- Extrinsic rewards reduce intrinsic interest
- The use of extrinsic rewards by parents is related to less generous and less intrinsically motivated behaviors by their children
- Extrinsic rewards can be controlling

Extrinsic motivation: figure out what we’re trying to optimize for, and then directly optimize for that. However, extrinsic motivation can have problems. In education, extrinsic rewards reduce intrinsic motivation of children to learn.

For tokenized ecosystems, we must be similarly careful. Extrinsic motivation works for some goals like “maximize security” or “maximize sharing of data”. But it can be dangerous in some places. Let’s say you’re building a decentralized reputation system. Directly tokenizing reputation would incentivize people to game their reputation for money, leading to all sorts of poor behavior.

One possible answer is for the system to support intrinsic motivation rather than extrinsic: filter out the bad actors with economic stake (TCR)

### Appendix: Related Articles & Media

- Talk emphasizing optimization [presentation](https://www.slideshare.net/TrentMcConaghy/tokens-and-complex-systems) [video](https://www.youtube.com/watch?v=Sm8j0u5NuGQ)

- Talk emphasizing complex systems [presentation + video](https://medium.com/abq-blockchain-community/talking-blockchain-ai-complex-systems-3c5a33676f85)

### Appendix: Related Efforts

- [twitter](https://twitter.com/search?src=typd&q=%23tokenengineering)
- [token engineering wiki](http://tokenengineering.net/start)

### Definitions

**Token-Curated Registries** are decentrally-curated lists with intrinsic economic incentives for token holders to curate the list’s contents judiciously.

**Pareto optimality** is a formally defined concept used to determine when an allocation is optimal.

### Links

- [A Crash Course in Mechanism Design for Cryptoeconomic Applications](https://medium.com/blockchannel/a-crash-course-in-mechanism-design-for-cryptoeconomic-applications-a9f06ab6a976)

- [Token-Curated Registries](https://medium.com/@ilovebagels/token-curated-registries-1-0-61a232f8dac7)

- [The Emergence of Cryptoeconomic Primitives - The building blocks of a token-based society](https://medium.com/@jacobscott/the-emergence-of-cryptoeconomic-primitives-14ef3300cc10)

- [Problems With Extrinsic Motivation](http://webshare.northseattle.edu/fam180/topics/praise-rewards/problems.htm)

## Part III - Token Engineering Case Studies

*Analysis of Bitcoin, Design of Ocean Protocol.*

### Case Study: Analysis of Bitcoin

**objective function**: Maximize the security of its network. It then defines “security” as compute power (hash rate), which makes it expensive to roll back changes to the transaction log. Its block reward function manifests the objective, by giving **block reward tokens (BTC)** to people who improve the network’s compute power.

`E(Ri) ~ Hi * T`

- E: Expected value ([probabilistic micro-payments](https://www.orchid.com/whitepaper.pdf))
- R: Block rewards
- H: Hash power of actor i (contribution to security)
- T: Tokens dispensed each block (12.5 BTC every ten minutes)

>"this higher variance is mitigated simply by higher level mining pools, which have the direct effect of reducing variance. This is cool because it means that Bitcoin doesn’t need even need to do that directly."
??

### Case Study: Design of Ocean Protocol

#### Introduction: Iterations

1. Write proper objectives and constraints.
2. Try plug-and-play patterns (solvers)
3. Update goals and back to step 2 until exhausted existing plug-and-play patterns
4. Design New Pattern

#### Problem Formulation

Recall that the objective function is about getting people to do stuff. First decide who those people are. For each one

- stakeholder/system agent: describe it by tasks they perform
- what value they can provide
- what they might get in return

**Objective function**: After the iterations described above, we arrived at an objective function of: maximize the supply of relevant AI data & services.

**Constraints** In the iterations described above, used this checklist when considering various designs.

- For priced data, is there incentive for supplying more? Referring? Good spam prevention?
- For commons (free) data, is there incentive for supplying more? Referring? Good spam prevention?
- Does the token give higher marginal value to users of the network versus external investors?
- etc...

Besides these questions, as we continually polled others about possible attacks; added each new concern to the list of constraints to solve for; and updated the design to handle it.

#### Exploring the Design Space

We tried a variety of designs that combined token patterns in various ways; and tested each design (in thought experiments) against the constraints listed above.

We needed to resort to step 3 of the methodology: design our own building block. What emerged was a Curated Proofs Market (CPM; next section has detail), a small-as-possible extension of a CM. We tried it in two new design options

#### A New Token Pattern for Ocean: Curated Proofs Markets

Ocean leaves curation to the crowd: users must “put their money where there mouth is” by betting on what they believe will be the most popular datasets, using a Curation Market setting.

Then we needed to reconcile signals for quality data with making data available. We resolved that by binding the two together: predicted popularity versus actual (proven) popularity.

Together, these form what we call a Curated Proofs Market (CPM). In a CPM, the curation market and the proof are tightly bound: the proof gives teeth to the curation, to make curation more action-oriented; in turn, the curation gives signals for quality to the proof. CPMs are a new addition to our growing list of token design building blocks

Ocean’s token rewards function:

`E(Rij) ~ log10(Sij) * log10(Dj) * T * Ri`

- E(Rij): Expected reward, for user i on dataset j
- Sij: Predicted popularity, user's curation market stake in service j (eg dataset j)
- Dj: proofed popularity, proof-of-service invoked (eg downloads of dataset j)
- T: tokens during interval
- Ri: to mitigate one particular attack vector

The [Ocean whitepaper](https://github.com/oceanprotocol/whitepaper/raw/master/whitepaper.pdf) elaborates on how this reward function works.

### Appendix: Related Efforts

[Prediction markets for content curation DAOs](https://ethresear.ch/t/prediction-markets-for-content-curation-daos/1312)

### Links

- [Orchid: Enabling Decentralized Network Formation and Probabilistic Micro-Payments](https://www.orchid.com/whitepaper.pdf)

- [Ocean Protocol withe paper](https://github.com/oceanprotocol/whitepaper/raw/master/whitepaper.pdf)

- [Token-Curated Registries 1.0](https://medium.com/@ilovebagels/token-curated-registries-1-0-61a232f8dac7)

- [Introducing Curation Markets: Trade Popularity of Memes & Information (with code)!](https://medium.com/@simondlr/introducing-curation-markets-trade-popularity-of-memes-information-with-code-70bf6fed9881)