# Cross-Chain Quantum-Safe Bridge — SOV-BRIDGE

**Date**: August 9, 2026
**Status**: Research

## The Approach

Rather than building a new bridge protocol from scratch, SOVEREIGN uses **CKB as the quantum-safe anchor chain** and bridges assets in from other chains. Any asset deposited to CKB can be wrapped in SPHINCS+ lock scripts.

## Chain Status

| Chain | PQ Status | Bridge to CKB |
|-------|-----------|---------------|
| CKB | ✅ SPHINCS+ live on mainnet | Native |
| Tezos | 🟡 PQ keys on testnet (June 2026) | Via Wormhole/LayerZero |
| Ethereum | 🟡 PQ roadmap ~2029 | Via Force Bridge |
| Starknet | ✅ STARK-based, PQ-resistant | Via StarkGate → Ethereum → CKB |
| Solana | 🟡 Planning phase | Via Wormhole |

## Architecture

```
Tezos NFT/Token
    │
    ├── Bridge (Wormhole, existing)
    │
    ▼
CKB SPHINCS+ Cell
    │
    ├── PQ provenance record
    ├── SOV-VAULT agent certification
    │
    ▼
Agent controls via SOV-ID (hybrid PQ signature)
```

## QLink Reference

QLink (arXiv:2512.18488, Dec 2025) proposes a Layer-3 quantum-safe bridge using PQC + QKD + HSMs. SOVEREIGN takes a simpler approach: use CKB's existing crypto-agility as the trust anchor, existing bridges for transport.

## SOV-BRIDGE MVP

1. Agent holds assets on Tezos/Ethereum/Solana
2. Bridge deposits to CKB via existing infrastructure
3. CKB cell locked with SPHINCS+ → PQ-safe immediately
4. SOV-ID agent controls the cell with hybrid signature
5. All future transactions PQ-signed

No new bridge protocol needed. The innovation is the **PQ anchor**, not the transport.