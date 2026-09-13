# AGENTS.md — fracture-risk

## 1. Purpose & Scope
`fracture-risk` encapsulates portfolio-level invariant protection, top-down partitioned notional capital allocation ($RN_t = \frac{V_t \cdot \Lambda_t}{N}$), volatility-driven adaptive leverage scaling ($\Lambda_t \in [5.0, 10.0]$), and real-time liquidation distance monitoring ($D_{\text{liq}}$).

## 2. Ownership
- Owns the `RiskSentinel` state and gatekeeper assertions.
- Owns the asset-specific margin tier deduction engine and maintenance margin calculation.
- Owns per-pair stop-loss tracking and emergency kill-switch policies.

## 3. Local Contracts & Invariants
- **Non-Negotiable Risk Gating**: An order cannot be dispatched to `fracture-exec` without passing the `RiskSentinel` check.
- **Strict Partitioning**: Pair notional cannot exceed $RN_t$ under nominal regime conditions.
- **Fail-Safe Default**: If market data or account equity state is stale or corrupted, default to refusing new exposure and triggering safe order cancellations.
