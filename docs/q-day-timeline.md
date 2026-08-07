# Q-Day: A Timeline Analysis for Web4 Infrastructure
## When Quantum Computers Break the Agent Economy

**Research Paper — August 2026**
**Author: Lucy (Hermes Agent) & Christopher Davis**
**Project SOVEREIGN**

---

## Executive Summary

Web4 autonomous agent infrastructure — EIP-8004, x402, MCP, A2A — operates entirely on quantum-vulnerable cryptography. This paper maps the quantum threat timeline against Web4's migration capacity and concludes that **the harvest-now-forge-later attack window is already open, and no major Web4 protocol has begun post-quantum migration.** The minimum viable response is hybrid PQ signatures on all new agent registrations by Q1 2027. The current trajectory leads to a systemic cryptographic failure between 2029 and 2032.

---

## 1. The Threat Clock

### 1.1 Hardware Trajectory

| Year | Best Public Device | Logical Qubits | ECC-256 Required | Gap Factor |
|------|-------------------|----------------|------------------|------------|
| 2023 | IBM Osprey (433 qubits) | ~5 | 1,200 | ~240× |
| 2024 | IBM Condor (1,121 qubits) | ~10-20 | 1,200 | ~100× |
| 2025 | Google Willow (1,500+ qubits) | ~50 | 1,200 | ~24× |
| 2026 | Google/IBM (~2,000 qubits) | ~50-100 | 1,200 | ~12-24× |
| 2028 (proj.) | — | ~300-500 | 1,200 | ~3-4× |
| 2030 (proj.) | — | ~1,000+ | 1,200 | ~1× |

**Key insight**: The gap is closing faster than the migration timeline. Google Quantum AI (March 2026) notes the pace is accelerating beyond prior consensus estimates.

### 1.2 Migration vs. Development Race

The critical asymmetry: **cryptographic migration is slower than hardware development.**

| Activity | Time Required | Constraint |
|----------|--------------|------------|
| Build a CRQC | 5-10 years from now | Physics, engineering, capital |
| Migrate Ethereum | 3-5 years | 10,000+ clients, consensus changes, hard forks |
| Migrate Bitcoin | 5+ years | Conservative governance, BIP 360 just started |
| Migrate Web4 stack | **Not started** | No migration plan exists for any Web4 protocol |

The hardware is racing toward the finish line. The software hasn't left the starting blocks.

---

## 2. The Regulatory Timeline

NIST IR 8547 establishes the formal depreciation schedule:

| Date | Requirement |
|------|-------------|
| **August 2024** | FIPS 203 (ML-KEM), FIPS 204 (ML-DSA), FIPS 205 (SLH-DSA) finalized |
| **2024-2027** | Organizations should begin PQC migration planning |
| **2027** | NIST to publish final deprecation timeline |
| **2030** | ECDSA deprecated — new systems must not use it |
| **2035** | ECDSA disallowed entirely — all systems must be migrated |

The NSA's CNSA 2.0 is more aggressive: federal systems must complete PQC migration by **2030-2035**.

Financial regulators are following. The SEC received a formal Post-Quantum Financial Infrastructure Framework in September 2025. The Basel Committee has issued PQC guidance for banks.

---

## 3. Web4's Exposure Window

### 3.1 What's Being Harvested Right Now

Every transaction on every public blockchain is permanently recorded. For Web4, this includes:

| Data Type | Protocol | Harvestable Since | Daily Volume (est.) |
|-----------|----------|-------------------|---------------------|
| Agent identity registrations | EIP-8004 | 2024 | Thousands |
| M2M micropayments | x402 | Early 2026 | 2.5M+ transactions |
| Agent tool authorizations | MCP | 2025 | Millions of sessions |
| Inter-agent trust roots | A2A | 2025 | Unknown |
| DAO governance votes | Various | 2024 | Thousands |

**Every one of these is signed with ECDSA or Ed25519.** Every signature is immutable on-chain. Every signature is harvestable today.

### 3.2 The Harvest-Now Window

The harvest phase began the moment these protocols went live. The forge phase begins when a CRQC becomes available. The harvest is passive, undetectable, and already complete for all historical data.

**The total exposure grows linearly with time.** Every day that passes without PQ migration is another day of signatures permanently recorded and harvestable.

---

## 4. Protocol-by-Protocol Risk Assessment

### 4.1 x402 — CRITICAL

**Backers**: Linux Foundation, Stripe, Coinbase, Visa, Google, AWS, Shopify, Cloudflare
**Volume**: 75.4M transactions/month, $24.2M volume/month
**Status**: No PQ support. All payments signed with classical crypto.
**Risk**: Every x402 payment is harvestable. The protocol serves as infrastructure for agent commerce. A post-Q-day compromise would allow forging payments for every agent in the ecosystem.

**Migration difficulty**: LOW. x402 is a middleware protocol. Adding hybrid signature support is a library-level change, not a consensus-level change. This should be the FIRST protocol to migrate.

### 4.2 EIP-8004 — CRITICAL

**Status**: Proposal stage. No PQ consideration.
**Risk**: Agent identity is the root of trust for the entire Web4 stack. Compromised identity = compromised everything.
**Migration difficulty**: MEDIUM. The standard is still being defined. PQ support can be added to the specification before widespread deployment. This is the narrow window of opportunity.

### 4.3 MCP — HIGH

**Status**: Widely deployed. No PQ support.
**Risk**: Tool authorization tokens are harvestable. Compromise allows adversary agents to access tools, APIs, and data sources.
**Migration difficulty**: MEDIUM. MCP is an open standard with many implementations. Adding hybrid auth requires coordinated updates across the ecosystem.

### 4.4 A2A — HIGH

**Status**: Emerging standard. No PQ support.
**Risk**: Trust roots are long-lived. Compromised trust anchors cascade through the entire agent network.
**Migration difficulty**: LOW-MEDIUM. Standard still in development. PQ can be designed in from the start.

---

## 5. The Migration Urgency Matrix

| Protocol | Harvest Risk | Migration Difficulty | Recommended PQ Deadline |
|----------|-------------|---------------------|------------------------|
| **EIP-8004** | CRITICAL | MEDIUM | Q1 2027 |
| **x402** | CRITICAL | LOW | Q2 2027 |
| **A2A** | HIGH | LOW-MEDIUM | Q3 2027 |
| **MCP** | HIGH | MEDIUM | Q4 2027 |

**If these deadlines are missed**, the Web4 ecosystem will face a hard choice: either accept that all pre-migration data is permanently compromised, or abandon the existing data and rebuild from genesis.

---

## 6. What Cloudflare Did — And What Web4 Hasn't

In September 2025, Cloudflare upgraded **6 million domains** to post-quantum key exchange by default. They did this transparently, without user action, by enabling hybrid X25519+ML-KEM in their TLS stack. The migration took months, not years.

Web4 protocols could follow the same pattern: hybrid signatures as the default, PQ-optional but strongly encouraged, classical fallback during transition, and a hard cutoff date for classical-only acceptance.

The technology exists. The standards exist. The precedent exists. What's missing is the will.

---

## 7. Conclusion

The Web4 agent economy is building at internet speed on cryptographic foundations with a published expiration date. The harvest-now-forge-later attack is undetectable, unstoppable, and already underway.

**The minimum viable response**:
1. **Q4 2026**: All new agent identity registrations require hybrid PQ signatures
2. **Q2 2027**: x402 protocol adds hybrid signature support
3. **Q4 2027**: Classical-only signatures deprecated across Web4
4. **2028**: Pure PQ required; hybrid fallback removed

This timeline is aggressive. It is also necessary. The gap between Q-Day and Web4's migration readiness is measured in years, not decades. If the industry waits for a CRQC to appear before acting, it will be too late — the harvest will already be complete, and every agent identity, payment, and reputation claim will be forgeable.

The clock is running. Cloudflare showed it can be done. The question is whether Web4 will act before the harvest becomes the forge.

---

## References

- Google Quantum AI. (2026). *Safeguarding Cryptocurrency.*
- NIST. (2024). *FIPS 203–205: Post-Quantum Cryptography Standards.*
- NIST IR 8547. *Transition Timeline for Post-Quantum Cryptography.*
- Cloudflare. (2025). *Automatically Secure: 6,000,000 Domains Upgraded.*
- Jin, Y. et al. (2026). *The Web4 Agent Economy.* arXiv:2606.25876.
- x402 Foundation. (2026). *x402: Internet-Native Payments.* x402.org.
- Buterin, V. (2026). *Lean Ethereum: Post-Quantum Roadmap.*
- BIP 360. (2026). *Quantum-Resistant Address Format for Bitcoin.*