# References & Sources

## Primary Source

| Ref | Title | Source | Access Date | Notes |
|-----|-------|--------|-------------|-------|
| **[CASE]** | AI Synergy Hackathon 2026 — Case 02: Agriculture: The Harvest Decision Window | Competition case file (ps.pdf) | 2026-10-14 | All simulated scenario evidence (E01–E06) sourced here. Numerical claims are competition materials, not verified real-world statistics. |

## Case Evidence Traceability

| Evidence ID | Claim | Case Reference |
|-------------|-------|----------------|
| E01 | Mandi quotes range ₹14–24/kg across 4 markets (same morning) | Case Evidence Pack — Price spread |
| E02 | 90 km market transport/loading removes ₹4–6/kg advantage | Case Evidence Pack — Transport |
| E03 | 2 of 5 villages report unreliable mobile data | Case Evidence Pack — Connectivity |
| E04 | Farmers trust local traders/neighbours > unfamiliar apps | Case Evidence Pack — Trust |
| E05 | Heavy rain possible within 36 hours | Case Evidence Pack — Weather |
| E06 | 8–15% post-harvest loss in oversupply weeks | Case Evidence Pack — Waste |

## External Sources

**None used in Version 1 evidence base.** All claims derive from the competition case file. Per case instruction: *"the numerical entries are competition materials and should not be presented as verified real-world statistics about a named organisation or community."*

## Potential Future Sources (Not Used in V1)

| Category | Candidate Sources | Verification Needed |
|----------|-------------------|---------------------|
| Official mandi prices | eNAM (https://enam.gov.in), state APMC portals | API access, coverage, freshness SLAs |
| Weather forecasts | IMD (https://mausam.imd.gov.in), OpenWeatherMap | District vs village resolution, update frequency |
| Transport costs | NHAI toll data, local transporter surveys | Volume/distance/shared-transport variation |
| Post-harvest loss | ICAR/CIPHET studies, state agriculture dept reports | Crop-specific, region-specific, avoidable portion |
| Farmer digital access | TRAI reports, GSMA Mobile Economy India, field surveys | Village-level connectivity, device penetration, language |

## Documentation References

| Document | Purpose |
|----------|---------|
| `docs/01-problem-framing.md` | Three framings, decision matrix, selection justification |
| `docs/02-evidence-ledger.md` | E01–E06, assumptions A01–A05, unknowns U01–U06 |
| `docs/03-problem-statement.md` | Final scoped problem, users, objectives, success criteria |
| `specs/01-solution-srs.md` | Solution concept, SRS (FR/NFR), implementation plan, data strategy |
| `specs/02-ai-human-model.md` | Responsibility separation, decision boundary, AI rules, connectivity strategy |
| `tests/acceptance-tests.md` | AT-01 to AT-06 definitions, fixtures, evaluation plan |
| `ai-trail/interactions.md` | 8 genuine AI interactions (AI-01 to AI-08) |
| `ai-trail/decision-index.md` | 8 decisions mapped to interactions & evidence |
| `ai-trail/existing-components.md` | All components declared New; prior work acknowledged |

## Competition Requirements Reference

| Requirement | Source | Addressed In |
|-------------|--------|--------------|
| Compare plausible problem framings | Case file Round 1 output | `docs/01-problem-framing.md` |
| Select specific problem, justify with evidence | Case file Round 1 output | `docs/03-problem-statement.md` |
| Propose feasible solution | Case file Round 1 output | `specs/01-solution-srs.md` |
| Define SRS & implementation plan | Case file Round 1 output | `specs/01-solution-srs.md` |
| Preserve AI reasoning trail | Case file final checklist | `ai-trail/interactions.md`, `ai-trail/decision-index.md` |
| Separate AI/human responsibilities | Case file final checklist | `specs/02-ai-human-model.md` |
| ≥3 acceptance tests | Case file final checklist | `tests/acceptance-tests.md` |
| Disclose pre-existing components | Case file final checklist | `ai-trail/existing-components.md` |
| Ownership check | Case file final checklist | Team confirmation in `ai-trail/existing-components.md` |

---

*All case-derived numerical values (₹14–24/kg, ₹4–6/kg, 2 of 5 villages, 36 hours, 8–15%) are explicitly labeled **simulated case evidence** throughout the submission.*