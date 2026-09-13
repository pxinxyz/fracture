# AGENTS.md — fracture-research

## 1. Purpose & Scope
`fracture-research` provides the Phase 3.5 execution & capacity reality simulation environment. It consumes historical tick-level market data, simulates order book queue mechanics, injects latency perturbations, and computes the empirical strategy capacity curve $E_{\text{net}}(V)$.

## 2. Ownership
- Owns historical data ingestion (QuickNode SQL Explorer, Parquet, tick CSVs).
- Owns the event-driven matching simulator (queue placement, fills, cancellations, slippage).
- Owns walk-forward validation and regime holdout analysis.
- Owns the empirical capacity curve calculation $E_{\text{net}}(V)$.

## 3. Local Contracts & Invariants
- **Strict No-Lookahead Guarantee**: Simulation must proceed strictly on event timestamps; feature extraction cannot access future ticks or post-event state.
- **Realistic Execution Costs**: Every trade simulation must apply tiered exchange fees, priority spend, spread crossing, slippage, and adverse selection.
- **Latency Perturbation Tests**: Strategy candidates must be evaluated across stressed latency distributions ($+25\text{ms}, +50\text{ms}, +100\text{ms}$) before approval.
