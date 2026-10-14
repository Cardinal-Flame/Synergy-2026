# KrishiVed Market Decision Support

**AI Synergy Hackathon 2026 — Round 1 Submission**

A human-centred AI system for transport-adjusted mandi comparison and weather-aware selling decisions.

---

## Selected Case

**Case 02 — Agriculture: The Harvest Decision Window**

Smallholder vegetable farmers must decide when and where to sell perishable crops. Prices differ across mandis, transport is expensive, connectivity is intermittent, and weather changes can shift harvest timing.

## Problem

Farmers lack a reliable way to compare expected **net proceeds** across mandis after accounting for transport and selling costs. A market offering ₹24/kg may net less than one offering ₹22/kg once transport costs are included.

## Solution (V1)

A transparent, deterministic net-proceeds comparison tool that:
1. Accepts crop, quantity, location, and selling window
2. Retrieves mandi quotes with source/timestamp and transport estimates
3. Calculates: `Net Proceeds = Quantity × (Price/kg − Selling Costs/kg)`
4. Presents ranked comparison with explanations, data-freshness warnings, and weather context
5. Leaves the final selling decision with the farmer

## Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| Deterministic engine for all arithmetic | LLMs cannot reliably do arithmetic; correctness is non-negotiable |
| AI only for explanations | Separates correctness from communication; prevents hallucinated numbers |
| Simulated data primary | Reproducible tests > unreliable live API demos |
| No "recommend" action | Farmer retains decision authority; case boundary forbids replacing authorised advice |
| All components New | Fresh repo; prior KrishiVed work = domain knowledge only |

## Repository Structure

```
├── SUBMISSION.md                    # Master submission document
├── README.md                        # This file
├── docs/
│   ├── 01-problem-framing.md        # 3 framings, decision matrix, selection
│   ├── 02-evidence-ledger.md        # E01-E06, assumptions, unknowns
│   ├── 03-problem-statement.md      # Final scoped problem
│   └── 04-executive-summary.md      # 1-page overview
├── specs/
│   ├── 01-solution-srs.md           # Solution concept, SRS, implementation plan
│   └── 02-ai-human-model.md         # AI/human responsibility separation
├── tests/
│   ├── acceptance-tests.md          # AT-01 to AT-06 definitions
│   ├── evaluation-plan.md           # Evaluation protocol
│   └── fixtures/                    # 6 test fixtures (simulated data)
├── ai-trail/
│   ├── interactions.md              # 8 genuine AI interactions
│   ├── decision-index.md            # 8 decisions mapped to interactions
│   └── existing-components.md       # Component disclosure
└── references/
    └── sources.md                   # Case traceability, competition requirements
```

## Quick Start for Judges

1. Read `SUBMISSION.md` for the complete picture
2. Check `docs/01-problem-framing.md` for problem-framing comparison
3. Check `docs/02-evidence-ledger.md` for evidence classification
4. Check `specs/01-solution-srs.md` for SRS and implementation plan
5. Check `ai-trail/decision-index.md` for the AI reasoning trail
6. Check `ai-trail/existing-components.md` for honest disclosure

## Status

**Round 1: Documentation-only submission.** No working application has been built. All acceptance tests are marked "Not run" until Build Round implementation.

## Team

KrishiVed Market Decision Support — AI Synergy Hackathon 2026
