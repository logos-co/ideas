---
id: 34
title: "A Web of Trust for Logos"
status: approved
author: "otherfren"
created: "2026-04-24"
source_issue: 34
---

# A Web of Trust for Logos

## Description
A peer-to-peer Web of Trust (WoT) as a foundational trust layer for the Logos stack - enabling OTC/P2P trade and future services like oracles or DAOs.
Users form nodes in a directed trust graph. Each rating is a signed score from the set `{-3, -1, +1, +3}` plus a short context string.
When two users have mutually rated each other positively, they share their ratings about common acquaintances.

**Bad actors can be identified quickly even though no global graph ever exists** - every user sees the trust landscape from his own vantage point.

## Social Problem & Logos Stack Solution
Centralized reputation systems like eBay or Amazon are fundamentally broken in three ways:

1. **Sybil attacks**: a vendor with "4.99/5 from 5000 reviews" is meaningless if 4999 are fake and the one real interaction was a fraud.
2. **Censorship**: centralized operators can silently remove negative reviews, ban accounts, or bend rankings for paying customers.
3. **Metadata leakage**: a global adversary (state actor, platform operator) can enumerate who has a business relationship with whom - a massive privacy violation that chills trade.

Services like OTC crypto trading, oracle networks, and DAO voting weight all need some notion of trust between participants and - most importantly - a way to broadcast wrongdoing immediately. Without a trust layer native to Logos, every application has to reinvent this wheel - and in practice ends up rebuilding the same centralized, censorable, metadata-leaking systems.

The Logos stack enables a fundamentally different shape:
- **Logos Messaging (Waku)**: encrypted, metadata-light transport for exchanging signed ratings between trusted peers.
- **Logos Storage (Codex)**: durable, censorship-resistant cold backup of personal trust-graph state.
- **Keycard**: hardware-rooted identity so every rating is cryptographically bound to a real user and cannot be forged.

Because no global graph is ever assembled, a global adversary cannot enumerate "who trusts whom" at scale.
Each user only learns about peers they are already connected to, rendering privacy a structural property, not a policy.

## Previous Attempts & Hypotheses
**Centralized star ratings (eBay, Amazon, Airbnb)**
- Hypothesis: aggregated reviews give buyers confidence.
- Reality: Sybil attacks, pay-for-placement, and hidden moderation turn star counts into noise rather than signal.

**On-chain reputation tokens (soulbound tokens, POAPs, Lens)**
- Hypothesis: putting ratings on a blockchain makes them tamper-proof.
- Reality: ratings become public forever, destroying privacy; on-chain cost and latency make fine-grained interaction scoring impractical; the Sybil problem remains because identities are cheap.

**PGP-style web of trust (key-signing parties)**
- Hypothesis: users cryptographically vouch for each other's identities.
- Reality: binary trust / no-trust is too coarse; no native mechanism for negative ratings or context; UX is so poor that only a tiny community ever participated.

## Functional Specifications
**Functionality**
- Signed ratings: each rating is a Keycard-signed message `{rated_pubkey, score ∈ {-3,-1,+1,+3}, context_text, timestamp}`.
- Selective propagation: when user A and user B have each given the other a score ≥ +1, their clients (opt-in) exchange rating sets about common acquaintances over Logos Messaging.
- Local trust computation: each client computes a personal trust score for a target as a weighted walk over received ratings - e.g., "Alice trusts Bob because two people Alice trusts rate Bob +3."
- No global graph: ratings only travel between mutually-trusting peer edges, so no central index ever exists.

**Usability**
- Integrated into Logos chat / forum flows: one-click "rate this user" from any conversation or post, pre-filled with an abstract context snippet.
- Clear visual language for `+3 / +1 / -1 / -3` (four-tier endorsement / warning icons) instead of 5-star ambiguity.
- When the user encounters a new counterparty (e.g., for an OTC trade), the client surfaces "how do I know this person" - the shortest trust path through their own network.

**Reliability**
- Local-first: all ratings and derived scores live in a local database; the network is only for exchange.
- Local backup by default. Opt-in encrypted backup sharded across trusted peers: if the user reinstalls Logos and loses local state, the client queries peers for their encrypted backup shard and reconstructs the graph.

**Performance**
- Trust scoring is a bounded-depth graph walk (typically depth 2–3), cheap to compute locally even for thousands of known peers.
- Only diffs (new or updated ratings) are broadcast to trusted peers, not full snapshots.

**Supportability**
- Open protocol spec published alongside the reference client so third-party DAOs, oracles, and OTC markets can consume the same trust primitive.
- All cryptography reuses existing Logos primitives (Keycard signatures, Waku transport, Codex storage) - no bespoke crypto.

## Risks & Deployment Strategy
The core risk is the chicken-and-egg problem: A trust graph is useless until enough real users have rated each other. Early adopters see an empty graph and may leave because they see no purpose in it. Mitigation: Needs a real use case to bootstrap, like an OTC market plugin.

**Reference client**
- Objective: 
  - rating data model
  - signing
  - local trust computation
  - WoT member discovery and message exchanges
  - Resilient backup and recovery
    - manual backup/restore of local storage
    - opt-in encrypted backups stored with trusted peers
- Key Tech: Keycard-signed rating messages; local store; Waku; peer discovery mechanism
- Outcome: any two Logos users can rate each other and see derived trust paths in their local client. 

**OTC / P2P trade pilot**
- Objective:
  - embed WoT into an OTC trade flow to demonstrate concrete value
  - expose WoT as a consumable primitive for oracles, DAOs, and third-party dApps.
  - published protocol spec + client library
- Integration: counterparty discovery UI queries the local WoT before proposing a trade - "Do I know anyone who trusts this person?"
- Outcome: measurable fraud reduction on pilot OTC trades; first real seed of the network effect.