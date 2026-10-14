<!-- solve-problem:82ec8f0e-c00d-402c-ad85-1bd21814d306 -->
# Strategic block-relay service

A [Tangle Solve problem](https://solve.tangle.tools/problems/82ec8f0e-c00d-402c-ad85-1bd21814d306).

## What is the problem?

In Bitcoin-style proof-of-work networks, miners and nodes rely on relay networks (e.g., FIBRE, Falcon, earlier Bitcoin Relay Network) to propagate blocks quickly. These relay operators are *strategic*: they can (i) preferentially delay blocks for some clients, (ii) selectively forward (forward to some peers but not others), and (iii) maintain undisclosed private fast paths that give favored miners a propagation advantage. Faster propagation confers a real economic advantage — a miner whose blocks propagate faster suffers fewer orphans and, as Eyal–Sirer showed, can even profit by withholding blocks (selfish mining), so relay discretion translates into consensus power.

**Formal setting.** Model the relay as an intermediary between block producers and the network. Clients (miners) pay the relay for delivery. The relay observes a block and chooses a delivery time $t_i \ge 0$ and delivery set $S_i$ per client $i$, possibly holding private peering links of unknown latency. The goal: design a *mechanism* — contracts, payments, cryptographic/timing attestations — such that (a) the relay's equilibrium strategy is to deliver promptly and uniformly (strategyproofness against delay, omission, and hidden-topology deviations), and (b) compliance is *verifiable*, i.e., honest clients can detect or prove deviation (or deviations are unprofitable even if undetected). Assumptions to make explicit: network latencies are stochastic and unobservable to third parties; clocks are only loosely synchronized; relays may collude with subsets of miners. The precise open question: does a verifiable relay arrangement exist with these incentive properties under realistic latency uncertainty, and what is the formal model in which this can even be stated and proved?

## Why does it matters?

A solution would prevent relay operators from covertly transferring consensus power to favored miners — a real centralization vector, since a small number of relay networks carry most Bitcoin mining traffic. It would also instantiate the broader program of *verifiable network services*: generic mechanisms for proving an intermediary did not delay or drop traffic. If it stays open, relay operators retain an unauditable lever over block propagation, orphan rates, and hence effective mining rewards, and route-level attacks of the type Apostolaki et al. demonstrated remain hard to rule out at the incentive layer.

## What's already known?

- **Apostolaki et al. (2019), SABRE:** an infrastructure defense — a redundancy-protected relay *side network* with peering via multiple ASes, designed so no single AS partition can isolate it. It is robustness engineering, not mechanism design: it assumes relays are honest and addresses routing-layer adversaries, not strategic relay behavior. It offers no verifiability of relay conduct.
- **Eyal & Sirer (2013):** proves selfish mining is profitable below 50% when propagation is manipulable — establishing *why* propagation control is valuable, but assumes an honest network and analyzes the miner, not the relay.
- Related work on verifiable QoS (delay proofs, watchtowers, payment-channel service incentives) exists in networking and blockchain literature but (verify) no published mechanism handles all three deviations — delay, selective forwarding, hidden fast paths — simultaneously in a permissionless setting.

Gap: no formal model of a strategic relay with verifiable delivery, and no impossibility result either.

## What would count as solving it?

1. **Model:** A formal game-theoretic model of relay-mediated block propagation (players, information sets, latency distributions, payments).
2. **Positive result:** A mechanism (contract + cryptographic/timing scheme) and proof that, in equilibrium, prompt uniform delivery is optimal even with private fast paths — or a bound on the deviation gain.
3. **Negative result:** An impossibility theorem showing no such mechanism exists under unstabilizable latency uncertainty (e.g., undetectable delay below the network noise floor), which would itself be publishable and redirect effort to detection bounds.
4. **Intermediate:** Tight bounds on the maximum preferential delay a relay can inflict before detection, as a function of observation count and latency variance.

## How to start

1. **Read Eyal–Sirer §3–4** first to quantify how propagation advantage converts to mining profit — this is the relay's "budget" for bribes/deviation and sets the incentive constraint.
2. **Read SABRE (arXiv:1808.06254) §3 and threat model** to understand the existing infrastructure baseline and why honesty of relays is assumed there.
3. **Formalize the detection problem:** with stochastic latencies and unsynchronized clocks, derive the hypothesis-testing bound — how many observations are needed to distinguish preferential delay $\Delta$ from noise? This is the crux; everything else builds on it.
4. Consider combining **commit-then-reveal timing attestations** (clients sign receipt times, relay commits to topology) with a payment escrow slashed on statistically detected deviation; the main obstacle is that latency is exogenous and non-attributable — a relay can always blame the internet.

## Key references

- Apostolaki, Zohar, Vanbever, *SABRE: Protecting Bitcoin against Routing Attacks*, arXiv:1808.06254. Builds a partition-resistant relay side network for Bitcoin using AS-diverse peering and BGP-announced prefixes; an infrastructure defense, not incentive design. https://arxiv.org/abs/1808.06254
- Eyal, Sirer, *Majority is not Enough: Bitcoin Mining is Vulnerable*, arXiv:1311.0243. Shows selfish mining is profitable below 50% hashpower, with profitability improving as propagation to victims worsens — quantifying the value of propagation control. https://arxiv.org/abs/1311.0243
- (Suggested supplement, verify) Literature on verifiable delay / network delay measurement (e.g., delay-proof protocols, watchtower incentive designs) as the closest mechanism-design analogues for verifiable QoS.

## Warnings

- The census record is thin; this is a research *direction*, not a crisply stated conjecture. Expect to spend significant effort on formalization itself — that is legitimate contribution.
- Do not conflate the relay with the adversary in SABRE: SABRE's adversary is a network/router attacker; here the relay itself is the strategic party.
- Latency is noisy and non-attributable: any verifiability claim must survive the "relay blames the network" defense. Naive receipt-timestamp schemes fail without clock synchronization assumptions — state them explicitly.
- Beware collusion: a relay favoring one large miner may share the captured orphan-rate gain, so individual-rationality checks for the favored client matter.
- "Private fast paths" are hard to model: they are hidden topology, so the mechanism may need topology commitments or redundancy assumptions; flag which variant you solve.

---
*`BCGT-000263` · flagged for review*

## Acceptance guidance

1. **Model:** A formal game-theoretic model of relay-mediated block propagation (players, information sets, latency distributions, payments).
2. **Positive result:** A mechanism (contract + cryptographic/timing scheme) and proof that, in equilibrium, prompt uniform delivery is optimal even with private fast paths — or a bound on the deviation gain.
3. **Negative result:** An impossibility theorem showing no such mechanism exists under unstabilizable latency uncertainty (e.g., undetectable delay below the network noise floor), which would itself be publishable and redirect effort to detection bounds.
4. **Intermediate:** Tight bounds on the maximum preferential delay a relay can inflict before detection, as a function of observation count and latency variance.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/82ec8f0e-c00d-402c-ad85-1bd21814d306?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000263.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000263)
