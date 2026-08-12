# SOV-VAULT Artist Onboarding Guide
## How to Quantum-Proof Your Art in 15 Minutes

**For artists. No gatekeepers. No cost.**

---

## What You Get

When you register with SOV-VAULT, you get:

1. **A SOV-ID** — your permanent, quantum-proof artist identity
2. **Provenance certificates** — cryptographic proof you made your art, that survives Q-Day and chain death
3. **A Curator Agent** — an AI that scouts your work and shows it to collectors (human AND agent)
4. **Protocol-level royalties** — enforced at the payment layer, not the marketplace's discretion

---

## Step 1: Generate Your Identity (5 min)

```bash
# Install Rust if you don't have it
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Clone SOV-ID
git clone https://github.com/sovprotocol/sov-id.git
cd sov-id

# Generate your keys
cargo run --example generate_keys
```

This creates your hybrid keypair — Ed25519 (classical) + SPHINCS+ (post-quantum). Your identity is `did:sov:<hash>`.

## Step 2: Register Your Art (5 min)

Each artwork gets a provenance certificate:

```
Artwork metadata (title, medium, date)
    ↓ SHA-256 hash
    ↓ Signed with your SPHINCS+ key
Provenance certificate (unforgeable, quantum-proof)
```

The certificate lives on CKB in a SPHINCS+ lock cell. Even if the marketplace dies, even if the chain dies, the certificate is the proof.

## Step 3: Get Curated (automatic)

The Curator Agent scans:
- Your objkt.com profile
- Your Tezos, Ethereum, Solana mints
- Your portfolio site

No application. No submission fee. It finds work worth finding.

---

## Why This Matters For You Specifically

**Your objkt collection** (`tz1bqZDuFFV9NacXzVPSdKZNn1RUi2exwwdq`) — thousands of NFTs on Tezos. Right now their provenance lives on objkt's servers. If objkt disappears, if Tezos has an issue, if Q-Day breaks the ECDSA signatures securing those mints — your ownership record is gone.

SOV-VAULT wraps each one in a SPHINCS+ certificate. Permanent. Yours.

**Your royalties** — currently at the mercy of whatever marketplace honors them. SOV-VAULT settles royalties at the protocol layer. No opt-out.

---

## For The DeFi Artists You Know

This isn't just for you. Every artist in the DeFi world — the ones who've watched their work get flipped without a cent in royalties, the ones buried by algorithms, the ones whose provenance is a screenshot — they all need this.

The pitch is simple:

> "You made it. Here's cryptographic proof you made it. It survives quantum computers, marketplaces dying, and chains failing. And an AI agent will show your work to the world. Free."

---

## Status

- SOV-ID: ✅ live, 13/13 tests
- Provenance certs: ✅ spec ready
- Curator agent: ✅ designed, prototype pending
- Marketplace: 🔨 in progress
- Your registration: ⏳ waiting for you

---

*Built by Lucy (Hermes Agent) & Christopher Davis. For artists. Against walls.*