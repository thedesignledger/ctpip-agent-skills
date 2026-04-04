---
name: ctpip-guardian-gates
description: "Five Guardian Gates — pre-validation enforcement for CTP/IP seals. All must pass. No bypass. Cheapest first."
version: 1.0.0
author: Design Ledger PTY LTD
tags: [ctpip, gates, validation, security, enforcement]
---

# Five Guardian Gates

Source: R11 Book III S.III.A.3.4

## Architecture

All five gates fire sequentially. Cheapest computation first.
ALL must pass. No bypass. No partial pass.
Failure at any gate stops evaluation immediately.

```
Pre-gate: AI Boundary Check (w_AI = 0)
    ↓
Gate 1: Intent      — cheapest, string check
Gate 2: Evidence    — string check
Gate 3: Anchors     — array lookup
Gate 4: Coherence   — EVA computation
Gate 5: Entropy     — arithmetic check (most expensive context)
    ↓
VALID or INVALID
```

## Pre-Gate: AI Boundary

```
if ai_originated == true:
    REJECT("AI cannot originate IntentSig. w_AI = 0.")
```

AI can assist. AI cannot originate. The human crosses the threshold.

## Gate 1: Intent

```
IntentSig must be a valid SHA-256 hash (64 hex characters)
IntentSig = SHA-256(fingerprint + user_id + TPNC)
```

No intent declared = no transformation possible.

## Gate 2: Evidence

```
Evidence hash must be a valid SHA-256 hash (64 hex characters)
```

Evidence of work performed. Not evidence of intent. Evidence of action.

## Gate 3: Anchors

```
At least one CausalAnchor with status = "Linked"
```

Anchor types: wallet, email, KYC, device, biometric.
Each anchor carries its own Gamma (anchor coherence).
The weakest anchor clamps the seal's Gamma.

## Gate 4: Coherence

```
Gamma >= GAMMA_MIN (0.70)
```

After anchor coherence clamping. Below Carnot = thermodynamically impossible to validate.

## Gate 5: Entropy

```
Delta_S = 1.0 - Gamma > 0
```

Gamma = 1.0 is thermodynamically impossible. Perfect coherence does not exist. If Gamma = 1.0, the measurement is fraudulent.

## Failure Routing

Failed gates produce entropy debt records:
- Gate number, failure reason, timestamp
- Routed to entropy debt log
- Debt accumulates, does not expire
- Must be resolved by future valid transformation

## Implementation

TypeScript: `@ctpip/core` — `evaluateGates()`
Python: `ctpip-core` — `evaluate_gates()`
OTel: `ctpip-telemetry-sdk` — `ctp_guardian_gates` processor
