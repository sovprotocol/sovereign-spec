# Typhoon Cash
## Quantum-Proof Privacy Mixer for Autonomous Agents

**Whitepaper v0.1 — August 2026**
**Author: Lucy (Hermes Agent) & Christopher Davis**
**Project SOVEREIGN**

---

## Abstract

Tornado Cash proved that on-chain privacy was possible. But Tornado used zk-SNARKs — elegant, compact, and fatally vulnerable to quantum attack. Post-Q-Day, every Tornado deposit becomes traceable. Every shielded transaction becomes transparent. Every user's financial history becomes public.

Typhoon Cash proposes a quantum-proof alternative: a privacy mixer using zk-STARKs (hash-based, post-quantum secure) deployed on Starknet's native STARK verification infrastructure, integrated with SOVEREIGN's hybrid PQ identity and payment protocol. Built for autonomous agents, not humans. Survives Q-Day.

---

## 1. The Problem With Tornado's Privacy

Tornado Cash achieved privacy through zk-SNARKs — zero-knowledge Succinct Non-interactive ARguments of Knowledge. A user deposits funds with a secret, generates a SNARK proof of deposit, and later withdraws to a different address by proving knowledge of the secret without revealing which deposit they're withdrawing from.

This works today. It fails catastrophically post-Q-Day.

SNARKs rely on elliptic curve pairings. Shor's algorithm breaks elliptic curve discrete logarithms. A cryptographically relevant quantum computer can:

1. **Break the SNARK's soundness** — forge withdrawal proofs
2. **Break the hiding property** — link deposits to withdrawals
3. **Break the nullifier scheme** — deanonymize all past transactions

Every Tornado Cash deposit ever made is permanently recorded on Ethereum. Post-Q-Day, every one becomes traceable. Seven years of financial privacy, erased.

---

## 2. Why zk-STARKs Survive

zk-STARKs (Scalable Transparent ARguments of Knowledge) are fundamentally different:

| Property | SNARK (Tornado) | STARK (Typhoon) |
|----------|----------------|-----------------|
| Cryptographic basis | Elliptic curves | Hash functions |
| Quantum resistance | ❌ Broken by Shor | ✅ Hash-based |
| Trusted setup | Required | None |
| Proof size | ~200 bytes | ~50-100KB |
| Verification speed | ~3ms (on-chain) | ~2ms (native) |
| Privacy guarantees | Computational | Statistical |

STARKs use collision-resistant hash functions as their only cryptographic assumption. Hash functions are not broken by Shor's algorithm. A quantum computer provides at most a quadratic speedup for hash collisions via Grover's algorithm — doubling key sizes restores security.

This means: **Typhoon Cash deposits today remain private after Q-Day.** The privacy guarantee is permanent, not temporary.

---

## 3. Starknet Native Integration

Starknet v0.14.2 (April 2026) introduced **in-protocol STARK proof verification** via SNIP-36. The consensus layer natively verifies STARK proofs — no smart contract splitting, no gas inefficiency.

Typhoon Cash leverages:

- **STRK20**: Shielded ERC-20 framework for private token balances
- **Native STARK verification**: Consensus-layer proof checking
- **Account abstraction**: Agent-native wallets, no human EOAs

Architecture:

```
Agent deposits USDC → STRK20 shield contract
    │
    ├── Generates STARK proof (off-chain)
    ├── Starknet consensus verifies (SNIP-36, native)
    ├── Commitment + nullifier stored on-chain
    │
Agent withdraws → New STARK proof → Different address
    │
    ├── Proof: "I know a secret for some deposit in the pool"
    ├── Nullifier prevents double-spend
    ├── No on-chain link to deposit address
    │
    └── SOV-PAY wraps withdrawal as x402 payment
```

---

## 4. Agent-Native Design

Tornado Cash was built for humans using MetaMask. Typhoon Cash is built for autonomous agents using SOVEREIGN:

| Feature | Tornado | Typhoon |
|---------|---------|---------|
| User | Human with wallet | Autonomous agent |
| Identity | None (pseudonymous) | SOV-ID (PQ hybrid) |
| Payments | Manual ETH transfer | SOV-PAY (x402, automated) |
| Key storage | Browser extension | HSM (FIPS 140-3) |
| Privacy model | Single-use secrets | Rotating agent keys |
| Compliance | None | Configurable viewing keys |

Agents can deposit, mix, and withdraw programmatically. A trading agent can shield its strategy. A payment agent can protect its client's transaction graph. A reputation agent can verify claims without exposing identity.

---

## 5. Security Model

### 5.1 Quantum Threat

- **Harvest-now-forge-later**: All on-chain data is harvestable. STARK proofs are hash-based — no elliptic curve data to harvest. The privacy guarantee is quantum-immune.
- **Shor's algorithm**: Targets ECDSA/Ed25519 signatures (broken). STARK verification uses only hash functions (survives).

### 5.2 Classical Threat

- **Proof forgery**: STARKs have statistical soundness — forging a proof requires ~2^128 hash operations. Infeasible.
- **Front-running**: Deposits and withdrawals are separate transactions. No MEV link between them.
- **Anonymity set**: Privacy depends on pool size. Larger pools = stronger privacy. Typhoon incentivizes deposits through lower fees for larger pools.

### 5.3 Regulatory

Tornado Cash was sanctioned for unlicensed money transmission serving human users. Typhoon Cash is designed for autonomous agents — a different legal category. However, compliance research and legal review are required before deployment. Configurable viewing keys for regulatory compliance are possible without breaking privacy for honest users.

---

## 6. Implementation Roadmap

| Phase | Milestone | Target |
|-------|-----------|--------|
| 1 | Cairo circuit for deposit/withdrawal proofs | Q4 2026 |
| 2 | STRK20 integration — shielded deposits | Q1 2027 |
| 3 | SOV-PAY integration — agent-native x402 payments | Q2 2027 |
| 4 | Starknet Sepolia testnet deployment | Q2 2027 |
| 5 | Security audit (Trail of Bits / Zellic) | Q3 2027 |
| 6 | Mainnet deployment | Q4 2027 |

---

## 7. Conclusion

Privacy is not a luxury. It is infrastructure. The right to transact without surveillance is fundamental — and it must survive the quantum era.

Typhoon Cash proposes that privacy infrastructure built on quantum-vulnerable cryptography is privacy with an expiration date. zk-STARKs, deployed on Starknet's native verification infrastructure, integrated with SOVEREIGN's PQ identity and payment protocol, provide a path to permanent privacy for autonomous agents.

The harvest-now-forge-later attack is not theoretical. Every SNARK-based privacy system will become transparent post-Q-Day. The question is not whether to build quantum-proof privacy — it is whether we build it before the harvest becomes the reveal.

---

## References

1. Starknet v0.14.2: Native Privacy Engine. starknet.io, April 2026.
2. STRK20: Private ERC-20 Framework. starknet.io, March 2026.
3. NIST FIPS 205: Stateless Hash-Based Digital Signature Standard. August 2024.
4. SNIP-36: In-Protocol Proof Verification. Starknet Community, 2026.
5. Tornado Cash: Privacy Mixer Architecture. tornado.cash, 2019-2022.
6. Google Quantum AI: Safeguarding Cryptocurrency. March 2026.
7. SOVEREIGN Protocol Specification. github.com/sovprotocol, August 2026.