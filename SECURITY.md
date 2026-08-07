# Security Model — Project SOVEREIGN

## Signature Scheme

SOVEREIGN uses **hybrid signatures**: Ed25519 (classical) + SLH-DSA-SHA2-256s (FIPS 205, SPHINCS+).

### Why Hybrid

| Attack Surface | Ed25519 | SLH-DSA | Hybrid |
|---------------|---------|---------|--------|
| Classical computing | ✅ Secure | ✅ Secure | ✅ Secure |
| Quantum (Shor's algorithm) | ❌ Broken | ✅ Secure | ✅ Secure |
| Implementation bug in Ed25519 | ❌ Broken | ✅ Secure | ✅ Secure |
| Implementation bug in SLH-DSA | ✅ Secure | ❌ Broken | ✅ Secure |
| Side-channel attack on one scheme | Varies | Varies | ✅ Other scheme protects |

**Both signatures must pass.** If either fails, verification fails. This is verified by automated tests.

### Parameter Set

SLH-DSA-SHA2-256s (NIST Category 5 — 256-bit security). The strongest available parameter set. Hash-based, stateless, no known quantum or classical attacks.

### Signature Sizes

| Component | Size |
|-----------|------|
| Ed25519 signature | 64 bytes |
| SLH-DSA-SHA2-256s signature | 29,792 bytes |
| Public key (hybrid) | ~32 + ~64 bytes |

## Verified Attack Vectors

All verified by automated tests in `sov-id/src/security.rs`:

- ✅ **Downgrade to classical**: Hybrid mode rejects classical-only signatures
- ✅ **Downgrade to PQ**: Hybrid mode rejects PQ-only signatures
- ✅ **Replay attack**: Signatures are not replayable across messages
- ✅ **Cross-key forgery**: Different keypair's signature cannot verify
- ✅ **Mode confusion**: Classical mode rejects PQ-only; PQ mode rejects classical-only

## Open Questions

1. **Constant-time verification**: The `fips205` crate should be audited for timing side-channels.
2. **Formal verification**: The hybrid verification logic should be formally verified.
3. **Hardware signing**: SPHINCS+ signatures are ~30KB. Hardware acceleration for constrained devices.
4. **Post-quantum context binding**: Domain separation via context parameter `b""` — should be protocol-specific.

## Responsible Disclosure

Report vulnerabilities to: sovprotocol@proton.me

PGP key: [pending]

## Audit Status

- **Self-audit**: 13 automated tests covering all verification paths
- **External audit**: Not yet performed
- **Formal verification**: Not yet performed
- **CKB reference implementation**: The underlying SPHINCS+ lock scripts were audited by ScaleBit (December 2025)