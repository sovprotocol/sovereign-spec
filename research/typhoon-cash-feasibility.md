# Typhoon Cash — Feasibility Research
## Quantum-Proof Privacy Mixer for Autonomous Agents

**Date**: August 8, 2026
**Status**: Research complete — viable

## Core Finding

zk-STARKs are the only proving system in production today combining scaling, privacy, and quantum resistance. Starknet is actively quantum-proofing their infrastructure. This validates Typhoon Cash as architecturally viable.

## Architecture

### Deposit
1. Agent sends USDC to CKB SPHINCS+ commitment cell
2. Generates zk-STARK proof of deposit
3. Stores nullifier hash + commitment on-chain

### Withdrawal
1. Agent generates zk-STARK proof of valid commitment
2. Submits proof + nullifier + recipient address
3. Contract verifies STARK (PQ-resistant), releases funds
4. No link between deposit and withdrawal addresses

### Why STARKs not SNARKs

| Feature | SNARK (Tornado) | STARK (Typhoon) |
|---------|----------------|-----------------|
| Quantum resistance | ❌ Broken by Shor | ✅ Hash-based |
| Trusted setup | Required | None |
| Proof size | ~200 bytes | ~100KB |
| Verification | Fast | Fast |
| Production | Mature | Mature (Starknet) |

## Integration with SOVEREIGN

- Agent identity: SOV-ID (SPHINCS+ hybrid)
- Payment rail: SOV-PAY (x402 PQ)
- Privacy layer: Typhoon Cash (zk-STARK)
- Reputation: SOV-REP (PQ-signed claims)

## Starknet Compatibility

Starknet's July 2026 signal confirms they're building quantum-resistant infrastructure. Typhoon Cash could deploy on Starknet or as a standalone STARK verifier on CKB (SPHINCS+ native).

## Regulatory Note

Tornado Cash faced sanctions for unlicensed money transmission. Typhoon Cash serves autonomous agents, not humans. Different legal category. But compliance research needed before deployment.

## Next Steps

1. Prototype zk-STARK circuit for deposit/withdrawal
2. Integrate with SOV-PAY for agent-native payments
3. Deploy STARK verifier on CKB testnet
4. Legal review for agent-only mixer classification