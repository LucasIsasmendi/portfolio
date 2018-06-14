# Token Engineering Series - Oceanprotocol

## Part I - Can Blockchains Go Rogue?

*AI Whack-A-Mole, Incentive Machines, and Life.*

[Trent McConaghy - oceanprotocol](https://blog.oceanprotocol.com/can-blockchains-go-rogue-5134300ce790)

“Show me the incentive and I will show you the outcome.”

"Blockchain Superpower: Get people to do stuff by rewarding with tokens"

## Part II - Towards a Practice of Token Engineering

*Methodology, Patterns & Tools.*

[Trent McConaghy - oceanprotocol](https://blog.oceanprotocol.com/towards-a-practice-of-token-engineering-b02feeeff7ca)

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

**ALL OF IT**

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

> - Extrinsic rewards do not produce permanent changes
> - Extrinsic rewards reduce intrinsic interest
> - The use of extrinsic rewards by parents is related to less generous and less intrinsically motivated behaviors by their children
> - Extrinsic rewards can be controlling

Extrinsic motivation: figure out what we’re trying to optimize for, and then directly optimize for that. However, extrinsic motivation can have problems. In education, extrinsic rewards reduce intrinsic motivation of children to learn.

For tokenized ecosystems, we must be similarly careful. Extrinsic motivation works for some goals like “maximize security” or “maximize sharing of data”. But it can be dangerous in some places. Let’s say you’re building a decentralized reputation system. Directly tokenizing reputation would incentivize people to game their reputation for money, leading to all sorts of poor behavior.

One possible answer is for the system to support intrinsic motivation rather than extrinsic: filter out the bad actors with economic stake (TCR)

## Appendix: Related Articles & Media

- Talk emphasizing optimization [presentation](https://www.slideshare.net/TrentMcConaghy/tokens-and-complex-systems) [video](https://www.youtube.com/watch?v=Sm8j0u5NuGQ)

- Talk emphasizing complex systems [presentation + video](https://medium.com/abq-blockchain-community/talking-blockchain-ai-complex-systems-3c5a33676f85)

### Definitions

**Token-Curated Registries** are decentrally-curated lists with intrinsic economic incentives for token holders to curate the list’s contents judiciously.

### Links

- A Crash Course in Mechanism Design for Cryptoeconomic Applications [link](https://medium.com/blockchannel/a-crash-course-in-mechanism-design-for-cryptoeconomic-applications-a9f06ab6a976)

- Token-Curated Registries [link](https://medium.com/@ilovebagels/token-curated-registries-1-0-61a232f8dac7)

- The Emergence of Cryptoeconomic Primitives - The building blocks of a token-based society [Jacob Horne - coinbase](https://medium.com/@jacobscott/the-emergence-of-cryptoeconomic-primitives-14ef3300cc10)

- Problems With Extrinsic Motivation [link](http://webshare.northseattle.edu/fam180/topics/praise-rewards/problems.htm)

## Part III - Token Engineering Case Studies

*Analysis of Bitcoin, Design of Ocean Protocol.*
