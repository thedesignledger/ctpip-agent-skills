---
name: ctpip-lux-runtime
description: "LUX Runtime — coherence measurement layer of CTP/IP. Contains the EVA computation (coherence, CTU, classification, temporal debt). The math that validates human transformation."
version: 1.0.0
author: Design Ledger PTY LTD
tags: [ctpip, lux, runtime, eva, coherence, gamma, ctu, measurement]
---

# LUX Runtime

The measurement layer of CTP/IP. LUX Runtime contains the EVA computation
that produces coherence from three input channels.

Source: R11 Book II S.II.6, Book III S.III.A.3.3

## Core Formula

```
Gamma = (E × V × A) / (tau + epsilon_0)
```

Where:
- E = Energy commitment [0, 1] — metabolic cost, effort invested
- V = Vector alignment [0, 1] — intent-to-action alignment
- A = Attention persistence [0, 1] — sustained engagement
- tau = Temporal friction [0, ∞) — resistance, delay, interference
- epsilon_0 = 1.0 — stability constant (prevents division by zero)

## CTU Generation

```
CTU = PHI × E × V × A
```

CTU (Causal Time Unit) is only generated when Coherence Index >= 0.70 AND Delta_S > 0.

PHI = 1.618033988749895 (Golden Ratio)

## Attention Computation

```
A = 1 / (1 + alpha × sigma²)
```

Where sigma² is the rolling variance of coherence over the calibration window (10 cycles).
alpha defaults to 8.0.

## Classification

```python
if gamma >= 0.95:    return ROOT
if gamma >= 0.8187:  return BLOOM
if gamma >= 0.70:    return SEED
return REJECTED
```

Thresholds are physics-derived:
- 0.70 = Carnot efficiency limit
- 0.8187 = Landauer erasure limit (1 - 1/e)
- 0.95 = Relativistic coherence

## Temporal Debt

When Coherence Index < GAMMA_MIN:
```
D = (V × E × A) / (Gamma + epsilon_0)
```

Debt accumulates. It does not expire. It must be resolved by future valid transformation.

## Properties

- Deterministic: same inputs always produce same outputs
- Side-effect free: computation changes no state
- Non-participating: the engine does not influence what it measures
- Binary: VALID or INVALID. No partial credit.
- O(1): 10 fixed-size arithmetic operations per evaluation

## Implementation

TypeScript: `@ctpip/core` — `evaluateEVA()`
Python: `ctpip-core` — `evaluate_eva()`
Go: `ctpip-telemetry-sdk` — `EvaluateEVA()`

## Invariants

1. No CTU without EVA validation
2. No system validates its own output
3. w_AI = 0 (AI-originated transformations earn zero weight)
