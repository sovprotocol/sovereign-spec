# Typhoon Cash on Starknet — Integration Research

**Date**: August 9, 2026
**Status**: Viable — clear integration path

## Critical Finding

Starknet v0.14.2 (April 2026) added **in-protocol STARK proof verification** via SNIP-36. The consensus layer natively verifies STARK proofs. Previously, STARK verification required splitting proofs across multiple transactions — prohibitively expensive. Now it's native and cheap.

## Why Starknet for Typhoon Cash

| Feature | Starknet | Other L2s |
|---------|----------|-----------|
| STARK verification | Native (consensus) | Smart contract (slow/expensive) |
| Privacy SDK | Public, STRK20 | None |
| Shielded tokens | STRK20, strkBTC | None |
| Quantum resistance | Hash-based STARKs | SNARK-based (vulnerable) |
| Agent compatibility | Account abstraction | Limited |

## Architecture v2

```
Agent deposits USDC → STRK20 shield contract (Starknet)
    │
    ├── Generates STARK proof of deposit
    ├── Starknet consensus verifies natively (SNIP-36)
    │
Agent withdraws → STARK proof → different address
    │
    └── SOV-PAY wraps x402 payment → agent pays privately
```

## STRK20 Integration

STRK20 framework allows any ERC-20 token to be used privately with encrypted balances. Typhoon Cash would:
1. Accept deposits as STRK20-shielded tokens
2. Pool deposits in a single shielded contract
3. Allow withdrawals with STARK proofs
4. No on-chain link between deposit and withdrawal

## Next Steps

1. Study STRK20 contract interface
2. Prototype deposit/withdrawal circuit in Cairo
3. Integrate SOV-PAY for agent-native x402 payments
4. Deploy to Starknet testnet (Sepolia)
5. Legal review for agent-only mixer