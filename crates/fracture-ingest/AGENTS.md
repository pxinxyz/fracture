# AGENTS.md — fracture-ingest

## 1. Purpose & Scope
`fracture-ingest` manages high-throughput, low-latency market data streams across two primary sources:
1. Binance USDⓈ-M futures WebSockets (`@bookTicker` and `@aggTrade`) as external price discovery oracles.
2. Hyperliquid order book data via QuickNode binary gRPC (`StreamL2Book`, `StreamL4Book`, `ORDER_PRIORITY`) and local non-validating HyperCore node feeds.

## 2. Ownership
- Owns network transport connections (WebSocket, gRPC, local IPC).
- Owns dual-timestamping for accurate latency measurement (exchange event time vs local arrival time).
- Owns the local L2/L4 order book state machine with authoritative replacement snapshots and diff reconciliation.

## 3. Local Contracts & Invariants
- **Zero Lock Contention**: Network ingestion threads stream into lock-free SPSC event rings; workers never block the I/O event loop.
- **Failover Resilience**: Seamless automated reconnection with exponential backoff on network disconnections.
- **Strict Timestamping**: Every incoming market packet is tagged with local monotonic hardware timestamps upon ingress.
