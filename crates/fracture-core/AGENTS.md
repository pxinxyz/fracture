# AGENTS.md — fracture-core

## 1. Purpose & Scope
`fracture-core` houses fundamental data structures, domain types, mathematical invariants, POMDP state models, and fixed-point price/size encodings shared across the entire Fracture workspace.

## 2. Ownership
- Owns core numerical embeddings and domain primitives (e.g. `Price`, `Size`, `InstrumentId`, `BookLevel`).
- Owns the POMDP state and observation formulations.
- Owns zero-allocation in-memory event primitives and ring buffer interfaces.

## 3. Local Contracts & Invariants
- **Zero Allocations on Hot Path**: Primitives must be stack-allocated, `Copy`, or bounded fixed-size buffers.
- **Fixed-Point Arithmetic**: Avoid IEEE-754 floating point imprecision for prices, sizes, and notional calculations.
- **No Side Effects**: Pure computational logic, zero network or filesystem I/O.
