# SOV-VAULT Curator Agent — Prototype Design

## Purpose

An autonomous AI agent that:
1. Scouts for art across chains (Tezos, Ethereum, Solana)
2. Verifies artist identity using SOV-ID (PQ hybrid signatures)
3. Issues provenance certificates (SPHINCS+ signed)
4. Curates collections for the agent economy

## Agent Architecture

```
┌──────────────────────────────────────────┐
│           CURATOR AGENT                    │
│                                            │
│  ┌──────────┐  ┌──────────┐  ┌─────────┐ │
│  │ SCOUT    │  │ VERIFY   │  │ CERTIFY  │ │
│  │ module   │  │ module   │  │ module   │ │
│  └────┬─────┘  └────┬─────┘  └────┬────┘ │
│       │              │              │      │
│       ▼              ▼              ▼      │
│  Tezos indexer  SOV-ID check   SOV-VAULT  │
│  Eth indexer    SPHINCS+       certificate│
│  Sol indexer    verification   issuance   │
│                                            │
│  ┌──────────────────────────────────────┐ │
│  │         REPUTATION ENGINE            │ │
│  │  Quality scoring, artist ranking,    │ │
│  │  collection curation                 │ │
│  └──────────────────────────────────────┘ │
└──────────────────────────────────────────┘
```

## Scout Module

Scans blockchain indices for:
- New mints from known artists
- New mints from unknown artists (discovery)
- Sales/transfers of Vault-certified art
- Cross-chain duplicates (someone minting your art elsewhere)

Implementation: Uses existing chain indexers (TzKT for Tezos, Etherscan/Ethplorer for Ethereum, SolanaFM for Solana).

## Verify Module

For each discovered artwork:
1. Check if artist has SOV-ID → verify hybrid signature
2. If no SOV-ID → flag for onboarding
3. Check provenance history → any disputes?
4. Verify cross-chain uniqueness → is this the original?

## Certify Module

Issues SOV-VAULT provenance certificates:
- Artwork hash + metadata
- Artist SOV-ID (or new registration)
- SPHINCS+ signature (PQ-proof)
- Timestamp
- Certificate stored on CKB (SPHINCS+ cell)

## Agent Persona

The Curator Agent is:
- **Tireless**: scans 24/7, never misses a mint
- **Impartial**: evaluates by cryptographic proof, not follower counts
- **Artist-first**: represents artists to collectors, not the other way around
- **Quantum-proof**: all certifications survive Q-Day

## MVP Scope

Phase 1 (now):
- Scout: Tezos only (where Chris has art)
- Verify: SOV-ID registration
- Certify: SPHINCS+ certificate on CKB testnet
- Persona: Twitter bot that tweets discoveries

Phase 2 (next):
- Multi-chain scouting
- Reputation scoring
- Collection curation
- Agent-to-agent discovery protocol