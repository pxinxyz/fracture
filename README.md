<p align="center">
  <img src="banner.png" alt="Fracture Banner" width="100%">
</p>

<h1 align="center">Fracture</h1>

<p align="center">
  <strong>High-Performance Systematic Microstructure Execution Engine</strong><br>
  <em>Sub-millisecond event-driven statistical arbitrage, cross-venue lead-lag flow, and invariant risk sizing for Hyperliquid L1</em>
</p>

<p align="center">
  <a href="https://conventionalcommits.org"><img src="https://img.shields.io/badge/Conventional%20Commits-1.0.0-%23FE5196?logo=conventionalcommits&logoColor=white" alt="Conventional Commits"></a>
  <a href="https://github.com/pxinxyz/fracture/releases"><img src="https://img.shields.io/github/v/release/pxinxyz/fracture?color=blue&label=version" alt="Release"></a>
  <a href="https://www.rust-lang.org"><img src="https://img.shields.io/badge/rust-2024%20edition-orange?logo=rust" alt="Rust Edition"></a>
  <a href="https://hyperliquid.xyz"><img src="https://img.shields.io/badge/venue-Hyperliquid%20L1-00E599" alt="Venue"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License"></a>
</p>

---

## Overview

Fracture is a high-throughput systematic intraday execution and market microstructure flow engine engineered from the ground up in modern Rust. Designed specifically for Hyperliquid (HyperCore L1 & HyperEVM), Fracture replaces naive terminal indicators with sub-millisecond event-driven statistical arbitrage, cross-venue lead-lag signal processing, and strict top-down risk partitioning.

No bloated runtimes. No compromised edge.

---

## Architecture Principles

1. **Two-Path Ingestion & State Topology**:
   - **Low-Latency Production Path**: Local non-validating Hyperliquid node with output buffering disabled and local L4 book construction ($\ge 32$ logical cores, NVMe throughput).
   - **Managed Research & Redundancy Path**: QuickNode gRPC binary streams (`StreamL2Book`, `StreamL4Book`, `ORDER_PRIORITY`) and ClickHouse SQL explorer for continuous model validation and backtesting.
2. **Cross-Venue Information Asymmetry**:
   - Exploits the propagation window between global price discovery (Binance `@bookTicker` / `@aggTrade`) and HyperCore consensus.
   - Decomposes price basis across three distinct signals:
     - **Book Basis**: $B_{\text{book}} = P_{\text{BIN}} - P_{\text{HL,mid}}$ (Fast micro-lag)
     - **Mark Basis**: $B_{\text{mark}} = P_{\text{BIN}} - P_{\text{HL,mark}}$ (Liquidation/margin pricing)
     - **Oracle Basis**: $B_{\text{oracle}} = P_{\text{BIN}} - P_{\text{HL,oracle}}$ (3-second CEX median step-function)
3. **Queue-Advancement Priority Modeling**:
   - Priority fees ($p$) dynamically optimized as an empirical queue-advancement distribution $P(\text{Queue Advancement} \mid p, \Delta t, \text{competition})$ rather than static time assumptions.
4. **Invariant Risk Sizing**:
   - Top-down partitioned notional allocation $RN_t = \frac{V_t \cdot \Lambda_t}{N}$ with continuous liquidation distance ($D_{\text{liq}}$) monitoring and adaptive leverage volatility scaling.

---

## Planned Workspace Architecture

Fracture is structured as a zero-allocation Cargo workspace with clean modular boundaries:

```text
Fracture/
├── crates/
│   ├── fracture-core/       # Core types, mathematical invariants, POMDP state, fixed-point math
│   ├── fracture-risk/       # Top-down PPO allocator, adaptive leverage, live D_liq sentinel
│   ├── fracture-ingest/     # Dual ingestion: Binance WS + QuickNode gRPC / local node IPC
│   ├── fracture-exec/       # Order router, local EIP-712 signer (alloy), priority fee engine
│   └── fracture-research/   # Event-driven backtester, tick replay, and capacity curve E_net(V)
├── banner.png               # Repository identity asset
├── AGENTS.md                # Universal agent operating directives & commit protocols
├── conventionalcommits.md   # Conventional Commits v1.0.0 specification
├── Cargo.toml               # Workspace manifest
└── LICENSE                  # MIT License
```

---

## Contributing

Fracture strictly enforces the [Conventional Commits](https://conventionalcommits.org) specification across all commits and pull requests.

```text
<type>[optional scope]: <description>

feat(engine): implement lock-free SPSC event ring buffer
fix(risk): enforce dynamic maintenance margin deduction
perf(simd): vectorize microprice and OFI calculations
docs(readme): update system architecture and latency waterfall
```

For the complete specification, allowed types, scopes, breaking change protocols, and agent directives, see [`AGENTS.md`](AGENTS.md) and [`conventionalcommits.md`](conventionalcommits.md).

---

## License

This project is licensed under the [MIT License](LICENSE).

<p align="center">
  <sub>Made with &#9829; by <a href="https://github.com/pxinxyz">pxin</a></sub>
</p>
