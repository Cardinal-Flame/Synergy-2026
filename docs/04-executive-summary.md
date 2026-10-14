# Executive Summary

## Case & Problem
**Case 02 — Agriculture: The Harvest Decision Window.** Smallholder vegetable farmers lack a reliable way to compare expected net proceeds across mandis when quoted prices (₹14–24/kg simulated), transport costs (₹4–6/kg advantage erosion simulated), connectivity (2 of 5 villages unreliable), weather (heavy rain possible in 36h), and trust (farmers prefer local traders) all vary.

## Selected Problem
**Net Proceeds Comparison (Transport-Adjusted Mandi Selection).** Not price discovery (highest quote ≠ best outcome) nor harvest timing (requires crop-quality models beyond case evidence). Justified by E01+E02 direct evidence alignment, feasible deterministic core, and testable scope.

## Proposed Solution
**KrishiVed Market Decision Support** — A transparent decision-support tool that:
1. Accepts crop, quantity, location, selling window
2. Retrieves mandi quotes with source/timestamp, transport estimates, weather forecast
3. Calculates deterministic net proceeds: `Total = Qty × (Price/kg − Costs/kg)`
4. Presents ranked comparison with plain-language explanation, assumptions, data freshness, and uncertainty warnings
5. Leaves final selling decision with the farmer

## Key Technical Choices
- **Deterministic engine** for all arithmetic (correctness, reproducibility)
- **AI only for explanations** (receives validated JSON, preserves numbers/caveats)
- **Simulated data primary** (reproducible tests); live API = prototype tier
- **Explicit data-quality flags** (missing, stale, conflicting, offline) — no fabrication
- **English-only V1**; Hindi = future
- **Farmer decision authority** explicit — no "recommend" action

## Version 1 Scope
| Included | Excluded |
|----------|----------|
| Net-proceeds comparison for 1 crop, ≤5 mandis | Price prediction, harvest optimization |
| Deterministic calc + transparent assumptions | Autonomous decisions |
| Simulated dataset + prototype API tier | Nationwide coverage, multi-crop |
| Stale/missing/conflict data handling | SMS/IVR/WhatsApp/voice |
| Weather context (source, time, coverage) | Guaranteed outcomes |

## Verification
- 6 acceptance tests defined (AT-01 to AT-06) with fixtures
- All numerical claims labeled **simulated case evidence**
- AI reasoning trail: 8 genuine interactions (AI-01 to AI-08)
- Decision index: 8 consequential decisions traced to interactions
- Existing components: **all New** — fresh repo, no reuse

## Submission Readiness
- Problem-framing comparison with evidence-based justification ✅
- Evidence ledger separating simulated/assumed/unknown ✅
- SRS with FRs, NFRs, error handling, V1 boundary ✅
- AI/human responsibility separation ✅
- ≥3 acceptance tests with pass criteria ✅
- V1 exclusions explicit ✅
- Genuine AI logs + decision index ✅
- Honest existing-component disclosure ✅

**Target**: Round 1 documentation-only submission demonstrating evidence-backed problem selection, feasible design, verifiable requirements, and transparent AI-assisted reasoning.