# Problem-Framing Comparison

## Case Context
Case 02 — Agriculture: The Harvest Decision Window. Smallholder vegetable farmers deciding when and where to sell perishable crops. Complicated by mandi price differences, transport expenses, unreliable connectivity, weather changes, and trust in digital recommendations.

## Three Candidate Framings

| Framing | Central Question | Proposed Intervention | Key Case Evidence | Main Trade-off |
|---------|------------------|----------------------|-------------------|----------------|
| **A. Price Discovery** | Where are the highest quoted prices? | Display available prices at nearby mandis | E01: Quotes range ₹14–24/kg across 4 markets | Highest quote may not mean highest net proceeds (E02: transport removes ₹4–6/kg advantage) |
| **B. Net Proceeds Comparison** | Where could the farmer earn most after transport & selling costs? | Compare prices after estimated selling/transport costs | E01 + E02: Price spread + transport cost impact directly supports concrete selling decision | Requires reliable cost estimates and current quotes; cost variation by volume/shared transport unknown |
| **C. Harvest Timing / Perishability** | When should the farmer sell? | Compare sell-now vs delay scenarios using weather & quality models | E05: Heavy rain possible within 36 hrs; E06: 8–15% post-harvest loss in oversupply | Predicting quality loss & future prices reliably is difficult; local vs district forecast divergence unknown |

## Decision Matrix

Criteria (weighted by importance to Round 1 goals):

| Criterion | Weight | A. Price Discovery | B. Net Proceeds Comparison | C. Harvest Timing |
|-----------|--------|-------------------|---------------------------|-------------------|
| Farmer value (direct decision support) | High (30%) | 6 | **9** | 7 |
| Case evidence strength | High (25%) | 7 | **8** | 5 |
| Version 1 feasibility | High (20%) | 8 | **7** | 4 |
| Data availability / uncertainty | Medium (10%) | 7 | **6** | 3 |
| Safety & trust implications | High (10%) | 6 | **8** | 5 |
| Testability (acceptance tests) | Medium (5%) | 7 | **9** | 4 |
| **Weighted Score** | 100% | **6.8** | **8.1** | **4.9** |

Scoring scale: 1–10 (10 = best). Scores reflect team assessment, not experimental results.

## Selection & Justification

**Selected: Framing B — Net Proceeds Comparison (Transport-Adjusted Mandi Selection)**

**Rationale:**
1. **Scope control**: Price discovery alone (A) is too limited — highest quote ≠ best outcome. Harvest timing optimization (C) requires stronger evidence on crop-quality models, storage, spoilage, future prices, and weather-to-price mapping than the case provides.
2. **Evidence alignment**: E01 (price spread) + E02 (transport cost impact) directly support comparing net proceeds. This is the "concrete selling decision" the case evidence enables.
3. **Feasible V1**: A deterministic net-proceeds calculator with transparent assumptions can be built, tested, and explained within Round 1 scope.
4. **Extensible**: Weather awareness (E05), connectivity handling (E03), and trust via explainability (E04) become supporting features — not core optimization problems.
5. **Testable**: Clear acceptance tests (AT-01, AT-02, AT-03) with independently verifiable arithmetic.

**Supporting features in V1** (not core optimization):
- Weather signals flagged with source/timestamp/uncertainty (no harvest-timing recommendation)
- Cached data with freshness warnings for intermittent connectivity (E03)
- Plain-language explanations preserving numerical values and caveats (addresses E04 trust gap)

**Explicitly excluded from V1:**
- Guaranteed future price predictions
- Crop-quality degradation models
- Autonomous selling/harvest decisions
- Collective selling coordination