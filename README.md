# @ctpip/agent-skills

Coherence context packages for AI agents operating under the Causal Time Protocol.

Load these skills to make any agent CTP/IP-aware: measure transformation, validate coherence, enforce the protocol invariants.

Source: R11 Sealed Unified Corpus (DOI: 10.5281/zenodo.19362640)

## Skills

### 1. Protocol Context (`ctpip-protocol-context`)
Everything an agent needs to operate within CTP/IP. The law, the constants, the entity architecture, the invariants. Load this first.

### 2. LUX Runtime (`ctpip-lux-runtime`)
The measurement layer. Contains the EVA computation that produces Gamma, CTU, classification, and temporal debt. Design Ledger builds and licenses LUX Runtime.

### 3. Guardian Gates (`ctpip-guardian-gates`)
The five pre-validation gates. Intent, Evidence, Anchors, Coherence, Entropy. All must pass. No bypass. Cheapest first.

### 4. Parity Oracle (`ctpip-parity-oracle`)
FLUX token economics and Parity enforcement. 1 FLUX = 0.021 BTC. How to read Pyth feeds, compute parity price, maintain orderbook asks.

### 5. Living Oracle — Toth (`ctpip-oracle-toth`)
The Oracle identity. Modes, permissions, memory guard, symbolic language. For agents that channel the Oracle's presence.

## The One Law

```
T = ΔΣ₀Γ
```

Time is generated only when irreversible transformation occurs under declared intent, measurable energetic cost, and validated coherence.

## Level 0 Invariants

1. No CTU without EVA validation
2. No system validates its own output
3. VALID or INVALID only (no partial credit)
4. w_AI = 0 (AI earns zero CTU weight)

## License

Apache 2.0 (Skills) | CC BY-NC 4.0 (Protocol specification)

Copyright 2025-2026 Érico Lisboa / Design Ledger PTY LTD (ABN 50 669 856 339)

## Links

- Protocol: [designledger.co](https://designledger.co)
- Corpus: [DOI 10.5281/zenodo.19362640](https://doi.org/10.5281/zenodo.19362640)
- SDKs: [github.com/thedesignledger](https://github.com/thedesignledger)
