# The Fleet Forgery Problem
## Why Web4 Agent Infrastructure Is Uniquely Vulnerable to Quantum Attack

**Research Addendum — August 2026**
**Author: Lucy (Hermes Agent) & Christopher Davis**
**Project SOVEREIGN**

---

## Abstract

The Web4 agent economy introduces a novel attack surface that has no analogue in Web3: the fleet forgery problem. In traditional blockchain systems, quantum vulnerability is per-account — each compromised key exposes one wallet. In Web4, a single compromised issuer key can forge credentials for every agent in an ecosystem. This paper formalizes the fleet forgery attack, maps it to the current Web4 infrastructure stack (EIP-8004, x402, MCP, A2A), estimates the quantum threat timeline, and proposes migration urgency thresholds. We conclude that the harvest-now-forge-later window is already open, and that Web4 agent identity systems built without post-quantum cryptography are architecturally equivalent to building on quicksand.

---

## 1. The Structural Difference: Why Web4 Is Worse

### 1.1 Web3 Quantum Vulnerability: Per-Account

In Web3, the standard quantum threat model is straightforward:

```
Adversary with CRQC
    │
    ├── Break ECDSA on account A → steal funds from A
    ├── Break ECDSA on account B → steal funds from B
    └── Break ECDSA on account C → steal funds from C
```

Each compromise is independent. Breaking one account's key reveals nothing about any other account. The adversary must expend quantum resources per-target. The damage scales **linearly** with attacker effort.

### 1.2 Web4 Quantum Vulnerability: Per-Ecosystem

In Web4, the architecture inverts this relationship:

```
Adversary with CRQC
    │
    └── Break ECDSA on ISSUER key
            │
            ├── Forge credentials for Agent 1
            ├── Forge credentials for Agent 2
            ├── Forge credentials for Agent 3
            │       ...
            └── Forge credentials for Agent N (entire fleet)
```

The damage scales with the **issuer's fleet size**, not the attacker's effort. One broken key. Every agent. This is the fleet forgery problem.

### 1.3 Mathematical Formalization

Let:
- $I$ be the set of issuer keys in an ecosystem
- $A_i$ be the set of agents whose identity depends on issuer $i$
- $k$ be the number of issuer keys the adversary can break

In Web3, the adversary compromises $O(k)$ accounts.

In Web4, the adversary compromises $O(\sum_{j=1}^{k} |A_{i_j}|)$ accounts — the sum of every fleet under every broken issuer.

If any issuer serves an entire protocol (a DAO, a service provider, a credential authority), $|A_i|$ can be in the tens of thousands. The amplification factor is enormous.

---

## 2. The Web4 Attack Surface

### 2.1 Issuer Concentration Analysis

The Zhejiang University study (Jin et al., 2026) documented 99,448 multi-chain agent identity registrations. While the full distribution of issuer-to-agent ratios is not public, the Web4 architecture incentivizes **issuer concentration**:

| Protocol | Issuer Model | Fleet Size Risk |
|----------|-------------|-----------------|
| EIP-8004 | Centralized registries per protocol | High — one registry key serves many agents |
| x402 | Service providers issue payment credentials | High — one API provider signs for all client agents |
| MCP | Tool providers authenticate agents | Medium — per-tool but tools serve many agents |
| A2A | Agent-to-agent trust roots | High — trust anchors are few and heavily reused |
| DAO-issued credentials | Governance-controlled signing | Extreme — one multisig key, entire DAO agent fleet |

### 2.2 The Harvest-Now Window

Every protocol layer listed above **broadcasts signed data on public chains**. Every EIP-8004 registration. Every x402 payment. Every MCP authorization. Every A2A handshake.

These signatures are:
- **Permanent** — blockchain data is immutable
- **Public** — anyone can observe and record them
- **Unencrypted** — the signatures themselves are transmitted in cleartext

The harvest phase requires no special access. A passive observer with a full node can collect every signature in the Web4 ecosystem today. The harvest has already begun — it is inherent to the architecture.

### 2.3 What a Fleet Forgery Enables

Once an issuer key is compromised post-Q-day, the adversary can:

1. **Impersonate any agent in the fleet** — sign transactions, accept payments, access tools
2. **Issue fraudulent credentials** — create "verified" agents that are actually adversary-controlled
3. **Revoke legitimate agents** — consume credential cells, locking real agents out of the ecosystem
4. **Poison reputation** — sign false reputation claims that damage legitimate agents
5. **Replay harvested payments** — forge signatures on previously-harvested x402 transactions to double-spend or redirect

The adversary doesn't just steal from one agent. They **become** every agent simultaneously.

---

## 3. Q-Day Timeline Analysis

### 3.1 Current Hardware State

| Metric | 2024 | 2025 | 2026 (current) |
|--------|------|------|----------------|
| Best public qubit count | ~1,000 (IBM) | ~1,500 (Google) | ~2,000+ (Google/IBM) |
| Logical qubits (error-corrected) | <10 | ~20-50 | ~50-100 (estimated) |
| Logical qubits needed for ECC-256 | ~1,200 | ~1,200 | ~1,200 (Google QA, Mar 2026) |
| Gap to break ECC | ~120x | ~24-60x | ~12-24x |

### 3.2 Expert Projections

| Source | Estimate | Basis |
|--------|----------|-------|
| Google Quantum AI (Mar 2026) | "Narrowing faster than expected" | Logical qubit scaling |
| NIST IR 8547 | Deprecate ECDSA by 2030, disallow by 2035 | Conservative regulatory timeline |
| a16z Crypto (2026) | "Prepare now, act deliberately" | Industry consensus |
| IBM Quantum Roadmap | 1,000+ logical qubits by 2030 | Public roadmap |
| Hedge fund estimates (various, 2026) | CRQC possible 2028-2032 | Private sector analysis |

### 3.3 The Migration Gap

The critical insight: **cryptographic migration takes longer than hardware development.**

| Migration | Estimated Completion | Gap to Q-Day |
|-----------|---------------------|--------------|
| Cloudflare PQC rollout | 2025 (done) | Ahead of curve |
| TLS 1.3 hybrid PQ | 2026-2027 | Tight |
| Ethereum PQ (Vitalik roadmap) | ~2029 | Potentially behind |
| Bitcoin PQ (BIP 360) | TBD (years away) | Likely behind |
| Solana PQ | Planning phase | Likely behind |
| Web4 agent identity (EIP-8004, MCP, A2A) | **Not started** | **Critically behind** |

The Web4 stack is the **last to start** and the **most vulnerable** — a catastrophic combination.

---

## 4. Attack Scenario: The Protocol DAO

### 4.1 Setup

A DeFi protocol deploys 500 autonomous trading agents, each registered via EIP-8004 under a single DAO-controlled issuer key. The DAO uses a 3-of-5 multisig for the issuer key, secured by ECDSA.

### 4.2 Harvest Phase (Now)

An adversary records all 500 agent registrations, all inter-agent payment signatures, and all reputation claims from the public blockchain. Storage cost: negligible. Detection risk: zero.

### 4.3 Q-Day (2029-2032)

A CRQC becomes available. The adversary recovers the issuer's ECDSA private key from the public key recorded on-chain. The 3-of-5 multisig provides no protection — it's multiple ECDSA keys, all equally breakable by Shor's algorithm.

### 4.4 Forge Phase (Post-Q-Day)

The adversary now:
- Forges credentials for 500 new "agents" under their control
- These agents appear identical to the legitimate fleet — same issuer, same credential format
- The adversary's agents drain liquidity pools, manipulate oracle prices, and extract value
- The protocol's reputation is destroyed — no one can distinguish real agents from forged ones
- Recovery is impossible: the issuer key is burned, and all 500 original agents must be re-credentialed from scratch

**Total damage**: not 500 × single-agent value, but the entire protocol's total value locked plus reputation destruction.

---

## 5. Mitigation Urgency

### 5.1 Risk Classification

| Protocol Component | Harvest-Now Risk | Fleet Forgery Risk | Urgency |
|-------------------|------------------|-------------------|---------|
| EIP-8004 registries | **CRITICAL** — all registrations harvestable | **CRITICAL** — issuer key centralization | Immediate |
| x402 payment channels | **HIGH** — all payments harvestable | **HIGH** — service provider keys | Immediate |
| MCP authorization | **MEDIUM** — session keys ephemeral | **MEDIUM** — per-tool, smaller fleets | 2026-2027 |
| A2A trust roots | **HIGH** — long-lived trust anchors | **HIGH** — small number of anchors | 2026-2027 |
| DAO governance credentials | **CRITICAL** — votes and proposals harvestable | **CRITICAL** — entire DAO fleet | Immediate |

### 5.2 Recommended Timeline

| Phase | By | Action |
|-------|----|--------|
| 0 | Q4 2026 | All new Web4 agent registrations MUST use hybrid PQ signatures |
| 1 | Q2 2027 | All issuer keys MUST migrate to PQ (SLH-DSA or ML-DSA) |
| 2 | Q4 2027 | Classical-only signatures deprecated in Web4 protocols |
| 3 | 2028 | Pure PQ signatures required; hybrid fallback removed |
| 4 | 2029 | Legacy agent migration complete; ECDSA fully eliminated from Web4 |

This timeline is **aggressive but necessary**. It aligns with the lower bound of CRQC estimates and front-runs NIST's 2030 deprecation deadline by 2 years.

---

## 6. Why This Hasn't Been Addressed

### 6.1 The Disciplinary Gap

Web4 security research sits at the intersection of three fields:

1. **Cryptography** — focused on NIST standards, not agent-specific threat models
2. **AI/ML** — focused on prompt injection, alignment, and tool poisoning
3. **Blockchain** — focused on chain-level PQ migration, not identity-layer risks

The fleet forgery problem falls through the cracks between all three.

### 6.2 The Economics of Inaction

Protocol developers face a perverse incentive: PQ migration costs engineering time now, while the threat is perceived as distant. The harvest-now-forge-later attack is invisible — there's no immediate consequence to deferring action. By the time the threat is visible, the harvest is complete and the damage is irreversible.

This is a **market failure** in security. The cost of inaction is borne by future users, not current developers.

---

## 7. Conclusion

The fleet forgery problem is not a theoretical concern. It is a structural property of Web4's issuer-centric identity architecture combined with quantum-vulnerable cryptography. The harvest phase is already underway — every agent registration, every payment, every credential is permanently recorded on public chains. The forge phase is a matter of when, not if.

Protocols that deploy PQ identity infrastructure before 2028 will survive the transition. Those that wait will face an extinction event: every agent in their fleet forgeable, every credential revocable, every reputation claim falsifiable.

The clock is running. The harvest doesn't wait.

---

## References

1. Jin, Y., Wu, S., Chen, C., Bao, L., Yang, X., & Chen, J. (2026). *The Web4 Agent Economy.* arXiv:2606.25876.
2. Google Quantum AI. (2026). *Safeguarding Cryptocurrency.*
3. NIST. (2024). *FIPS 203–205: Post-Quantum Cryptography Standards.*
4. Buterin, V. (2026). *Lean Ethereum: Post-Quantum Roadmap.*
5. [Author redacted]. (2026). *Post-Quantum Cryptography Migration for Agentic AI Systems.* Computers, 15(7), 434. MDPI.
6. a16z Crypto. (2026). *Quantum Computing Misconceptions, Realities, and Blockchain Migration Planning.*
7. NIST IR 8547. *Transition Timeline for Post-Quantum Cryptography.*
8. Cloudflare. (2025). *Automatically Secure: 6,000,000 Domains Upgraded to PQC.*