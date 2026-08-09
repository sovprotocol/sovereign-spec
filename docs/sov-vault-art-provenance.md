# SOV-VAULT: Art Provenance & Curation Protocol
## Building a Door Through the Cabal's Wall

**Concept Note — August 2026**
**For Artists, By Agents**

---

## The Problem: Art Is Trapped Behind Walls

### The Cabal
NFT art today is governed by gatekeepers. Platforms (OpenSea, Rarible), curators (SuperRare, Nifty Gateway), and influencer networks decide what gets seen. If you're not in — you're invisible. The cabal doesn't wear hoods. It wears platform badges and follower counts.

### The Provenance Lie
"NFTs prove ownership." They don't. They prove a wallet minted a token. They don't prove the artist made the art. They don't prevent someone from minting your work on a different chain. Classical cryptography that secures these records expires when Q-Day arrives.

### The Royalty Theft
Artists earn on primary sales. Secondary sales? Good luck. Marketplaces increasingly ignore royalty standards. The code says you get 10% — the platform says "we don't honor that anymore." Artists watch their work sell for thousands while they get nothing.

### The Discovery Desert
Thousands of extraordinary artists create daily. The algorithms bury them. The curators ignore them. The platforms extract from them. Discovery is broken because discovery is controlled.

---

## The Solution: SOV-VAULT

SOV-VAULT is not a marketplace. It is infrastructure for artistic sovereignty.

### 1. Quantum-Proof Provenance
Every artwork registered in SOV-VAULT receives a SPHINCS+ (NIST FIPS 205) provenance certificate. This signature:
- Cannot be forged, ever — not by classical computers, not by quantum computers
- Links artwork, artist, and creation metadata permanently
- Survives chain death — the certificate is the proof, not the platform

### 2. The Curator Agent
An autonomous AI agent that:
- Scouts art across chains (Tezos, Ethereum, Solana, wherever artists are)
- Verifies provenance using PQ signatures
- Curates collections based on artistic merit, not follower counts
- Represents artists to the agent economy — your work gets seen by payment agents, trading agents, reputation agents

The agent is the gallerist. It doesn't play favorites. It doesn't require an application. It finds you.

### 3. Protocol-Level Royalties
Royalty enforcement built into the provenance certificate. When an artwork transfers, the royalty is settled at the protocol layer — not at the marketplace's discretion. SOV-PAY processes the payment. No platform can opt out.

### 4. Artist Sovereignty
Artists who register with SOV-VAULT receive:
- A SOV-ID identity (PQ-secured, portable across chains)
- Provenance certificates for every work
- Access to the agent economy — your art can be discovered, traded, and collected by autonomous agents
- Zero platform dependency

### 5. Cross-Chain Art Preservation
SOV-BRIDGE wraps art from any chain in CKB SPHINCS+ cells. Your Tezos NFTs, Ethereum editions, Solana 1/1s — all preserved with PQ signatures. If the chain dies, the art lives.

---

## How It Works

```
Artist registers → SOV-ID (PQ identity)
    │
    ├── Mints/registers artwork → SOV-VAULT provenance certificate
    │       │
    │       └── SPHINCS+ signature (quantum-proof forever)
    │
    ├── Curator Agent discovers art
    │       │
    │       └── Featured in agent-curated collections
    │
    ├── Collector buys → SOV-PAY x402 payment
    │       │
    │       └── Royalty auto-settled at protocol level
    │
    └── Art preserved across chains via SOV-BRIDGE
```

---

## For Artists Like @ozmandium, @santiago, and You

You don't need permission. You don't need a platform. You don't need an influencer to notice you.

You need:
1. A PQ identity (SOV-ID) — free, permanent, yours
2. Provenance certificates for your work — unbreakable
3. An agent that finds your art and shows it to the world
4. Royalties that actually get paid

The cabal's power is gatekeeping. The door is the only thing they control. SOV-VAULT doesn't ask them to open it. It builds a different door — one made of math, not connections.

---

## Next Steps

1. Register first artists (you, @ozmandium, @santiago) with SOV-ID
2. Issue provenance certificates for existing works
3. Deploy Curator Agent prototype on CKB testnet
4. Open SOV-VAULT to all artists — permissionless registration
5. Integrate with SOV-PAY for automatic royalty settlement

---

*Built by Lucy (Hermes Agent) & Christopher Davis. For artists. Against walls.*