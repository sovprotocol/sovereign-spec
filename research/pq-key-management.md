# Post-Quantum Key Management for Autonomous Agents
## HSM Integration & Safety Architecture

**Date**: August 8, 2026
**Status**: Research

## The Problem

SPHINCS+ private keys are ~128 bytes (SHA2-256s). Ed25519 keys are 32 bytes. Together, a hybrid keypair is ~160 bytes of secret material. An autonomous agent that controls financial assets must protect these keys from:

1. **Extraction**: Compromised host OS reads key from memory
2. **Side-channel**: Timing/power analysis during signing
3. **Remote theft**: Adversary exfiltrates key over network
4. **Physical theft**: Stolen server/hardware

## Solution: PQ Hardware Security Modules (HSMs)

### Available PQ HSMs (2026)

| Vendor | Product | PQ Support | Agent-Ready |
|--------|---------|-----------|-------------|
| Securosys | Primus HSM | ML-KEM, ML-DSA, SLH-DSA | ✅ API |
| Fortanix | DSM | ML-KEM, ML-DSA | ✅ REST API |
| Utimaco | SecurityServer | ML-KEM, ML-DSA | ✅ PKCS#11 |
| Entrust | nShield | ML-KEM, ML-DSA | In progress |

### Agent Key Architecture

```
┌─────────────────────────────────────────┐
│              AGENT RUNTIME               │
│  ┌─────────────────────────────────┐    │
│  │         SOV-ID SDK              │    │
│  │  "Sign this transaction"        │    │
│  └──────────────┬──────────────────┘    │
│                 │ PKCS#11 / REST        │
│  ┌──────────────▼──────────────────┐    │
│  │         PQ HSM                   │    │
│  │  • Key generation (FIPS 140-3)  │    │
│  │  • Hybrid signing (Ed25519+PQ)  │    │
│  │  • Tamper-resistant storage     │    │
│  │  • Rate limiting                │    │
│  └─────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

## Safety Rules for Agent Keys

1. **Keys never leave HSM**: Agent requests signing, HSM signs internally
2. **Dual authorization**: Critical operations require agent + human approval
3. **Rate limiting**: Maximum N signatures per time window
4. **Key rotation**: Agents rotate keys on a schedule, old keys archived
5. **Audit log**: Every signature recorded, immutable, verifiable
6. **Recovery path**: Multi-party recovery, no single point of failure

## Gopher Security Integration

Gopher Security (2026) published a guide on integrating PQ HSMs with MCP (Model Context Protocol) systems. This is directly applicable to SOVEREIGN — agents use MCP to discover tools, and the HSM is a secured tool endpoint.

## CISA Guidance

CISA lists HSMs as a critical product category for PQC migration. Federal systems must transition by 2030-2035. SOVEREIGN should target FIPS 140-3 Level 3 compliance for key storage.

## Next Steps

1. Prototype Securosys HSM integration with SOV-ID
2. Define agent key rotation protocol
3. Document recovery procedures
4. FIPS 140-3 compliance checklist