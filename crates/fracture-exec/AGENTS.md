# AGENTS.md — fracture-exec

## 1. Purpose & Scope
`fracture-exec` handles order serialization, sub-millisecond local cryptographic signing (EIP-712 / Secp256k1), persistent HTTP/2 transport to the HyperCore matching engine, dynamic priority fee optimization, and fast cancellation logic.

## 2. Ownership
- Owns API wallet management, cryptographic key storage, and nonce sequence integrity.
- Owns order construction (ALO, IOC, Market, Stop-loss).
- Owns priority fee calculation ($p$) based on expected alpha decay and queue advancement dynamics.
- Owns order cancellation dispatch and STP (Self-Trade Prevention) configuration.

## 3. Local Contracts & Invariants
- **Local Signing Only**: Private keys never leave memory; signing must occur locally in $< 200\mu\text{s}$.
- **Persistent HTTP/2 Egress**: Re-use open TCP/TLS connections with keep-alive to avoid connection handshake overheads on the critical dispatch path.
- **Nonce Monotonicity**: Ensure strict monotonic sequencing across concurrent order bursts without race conditions.
