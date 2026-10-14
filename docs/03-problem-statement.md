# Final Problem Statement

## Problem

**Smallholder vegetable farmers in the target cluster lack a reliable, accessible way to compare the expected net proceeds of selling a perishable crop across nearby mandis when quoted prices, transport costs, weather conditions, and connectivity vary.**

Existing information sources (market apps, word-of-mouth, trader quotes) do not consistently establish the price a particular farmer can actually obtain after selling expenses. Quoted mandi prices range widely (simulated: ₹14–24/kg), but transport and loading can remove ₹4–6/kg of a distant market's advantage. Connectivity is intermittent in some villages (2 of 5 report unreliable mobile data). Farmers trust local traders and neighbours more than unfamiliar digital recommendations. Weather changes (e.g., heavy rain within 36 hours) can affect crop quality and price, but district-level forecasts may not reflect local conditions.

This creates uncertainty around **where and when to sell**, leading to suboptimal decisions and potential income loss.

## Scope (Version 1)

**In scope:**
- Compare estimated net proceeds across a limited set of mandis for a single crop
- Transparent, deterministic calculation: `Net Proceeds = Quantity × (Quoted Price/kg − Selling Costs/kg)`
- Display quote source, timestamp, and freshness status
- Flag missing, stale, or conflicting data explicitly
- Show weather forecast with source, update time, and geographic coverage label
- Plain-language explanation of comparison, assumptions, and uncertainties
- Farmer retains final selling decision authority
- English-only UI; offline-capable with cached data warnings

**Out of scope (explicit V1 exclusions):**
- Guaranteed future price predictions or income increases
- Crop-quality degradation models or harvest-timing optimization
- Autonomous selling, harvesting, or logistics booking
- Collective selling coordination or group transport
- SMS, IVR, WhatsApp, or voice interfaces
- Hindi or other language support
- Nationwide mandi/crop coverage
- Claims of reduced post-harvest losses without evidence

## Users & Stakeholders

| Stakeholder | Role in V1 |
|-------------|------------|
| **Smallholder vegetable farmer (primary)** | Enters crop, quantity, location; reviews net-proceeds comparison; makes final selling decision |
| **Local aggregator / transporter (supporting)** | Validates transport-cost estimates; may assist farmer with app use |
| **Agriculture extension worker (supporting)** | Helps interpret weather/contextual information |
| **System administrator / data maintainer** | Configures data sources, freshness thresholds, simulated dataset |

## Measurable Objectives (V1)

1. **Correctness**: For any valid input set, the deterministic engine produces reproducible net-proceeds calculations (verified by AT-01, AT-02).
2. **Transparency**: Every displayed comparison shows quote source, timestamp, included costs, and material assumptions (verified by AT-03, AT-04, AT-05).
3. **Resilience**: Under connectivity loss, the system displays cached data with timestamps and does not present it as live (verified by AT-05).
4. **Understandability**: A farmer can identify the highest net-proceeds option and the key assumptions affecting it without technical terminology (evaluation plan).

## Success Criteria (Round 1)

- Problem-framing comparison documented with evidence-based justification
- Evidence ledger separates simulated case evidence, assumptions, and unknowns
- SRS defines functional/non-functional requirements, error handling, and V1 boundary
- ≥3 acceptance tests specified with pass/fail criteria and test fixtures
- AI reasoning trail records genuine interactions that shaped decisions
- Existing-component disclosure is honest (all components new for this submission)
- Master submission document (SUBMISSION.md) is coherent and internally consistent

## Evidence Base

This problem statement is justified by:
- **E01 + E02**: Price spread exists but transport costs change the ranking → net-proceeds comparison needed
- **E03**: Connectivity intermittent → offline/cache handling required
- **E04**: Trust gap → explanations must be transparent, farmer retains authority
- **E05**: Weather relevant but uncertain → flag with source/limitations, no automation
- **E06**: Perishability context → acknowledge but do not optimize without evidence
- **U01–U06**: Unresolved questions become V1 boundaries and explicit assumptions

All numerical values (₹14–24/kg, ₹4–6/kg, 2 of 5 villages, 36 hours, 8–15%) are **simulated case evidence** from the competition case file, not independently verified data.