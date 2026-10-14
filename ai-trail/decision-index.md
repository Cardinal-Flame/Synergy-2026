# Decision Index

Maps consequential design decisions to supporting AI interactions and verification evidence.

| Decision ID | Decision | Supporting AI Interactions | Evidence Checked | Final Outcome | Status |
|-------------|----------|---------------------------|------------------|---------------|--------|
| **D-01** | Selected **Net Proceeds Comparison (Framing B)** as primary problem over Price Discovery (A) or Harvest Timing (C) | AI-01, AI-02 | Case evidence E01+E02 directly support; decision matrix weighted scores (B=8.1, A=6.8, C=4.9) | **Confirmed** — documented in `docs/01-problem-framing.md` | Confirmed |
| **D-02** | Defined **Version 1 boundary**: deterministic net-proceeds comparison only; no harvest timing, no price prediction, no autonomous decisions, English only, simulated data primary | AI-03 | Feasibility analysis: deterministic engine testable in Round 1; harvest timing requires crop-quality models not in evidence | **Confirmed** — boundary table in `specs/01-solution-srs.md` Section 2.5 | Confirmed |
| **D-03** | **Deterministic calculation engine** for all arithmetic (net realization, totals, ranking, freshness); AI only for natural-language explanations | AI-04 | Manual verification: LLM arithmetic unreliable; explanation layer receives validated JSON only | **Confirmed** — architecture in `specs/01-solution-srs.md`, responsibility model in `specs/02-ai-human-model.md` | Confirmed |
| **D-04** | **Explicit missing/stale/conflict data handling**: flag, don't fabricate; offline shows cache with timestamp; no auto-resolution | AI-05 | Case evidence E01 (quote freshness unknown), E03 (connectivity), boundary (no fabrication) | **Confirmed** — FR-05, FR-09, AT-03/04/05 in `specs/01-solution-srs.md` & `tests/acceptance-tests.md` | Confirmed |
| **D-05** | **Farmer retains final selling decision authority**; system provides comparison + warnings + context, no "recommend" action | AI-04, AI-07 | Case boundary: "AI must not replace authorised advice"; trust evidence E04 (farmers trust traders > apps) | **Confirmed** — decision boundary diagram in `specs/02-ai-human-model.md` | Confirmed |
| **D-06** | **All components declared New** for this submission; prior KrishiVed work = domain knowledge only, no code reuse | AI-07 | Repo inspection: empty, fresh git history; no assets copied | **Confirmed** — `ai-trail/existing-components.md` | Confirmed |
| **D-07** | **Simulated data only for V1 correctness**; live API = prototype tier with clear labeling | AI-03, AI-08 | No live API access verified; reproducible test fixtures more valuable for Round 1 | **Confirmed** — data hierarchy in `specs/01-solution-srs.md` Section 4 | Confirmed |
| **D-08** | **6 acceptance tests** covering all Must-have FRs; marked "Not run" honestly | AI-06 | FR traceability matrix: AT-01/02→FR-03/04, AT-03/04/05→FR-05/09, AT-06→FR-06/07 | **Confirmed** — `tests/acceptance-tests.md` | Confirmed |

---

## Decision Traceability Matrix

| Requirement | Originating Decision | Verification Method |
|-------------|---------------------|---------------------|
| FR-01 (input acceptance) | D-01 (problem scope) | Input validation tests |
| FR-02 (quote display + provenance) | D-01, D-04 | AT-03, AT-04 |
| FR-03 (deterministic calculation) | D-03 | AT-01, AT-02 |
| FR-04 (net-proceeds comparison) | D-01, D-03 | AT-01, AT-02 |
| FR-05 (data quality flags) | D-04 | AT-03, AT-04, AT-05 |
| FR-06 (explanation) | D-03, D-05 | AT-06, consistency check |
| FR-07 (weather context) | D-01 (supporting feature) | Manual inspection |
| FR-08 (English UI) | D-02 (V1 boundary) | Language test |
| FR-09 (cached data warnings) | D-04 | AT-05 |
| NFR-01 (correctness) | D-03 | AT-01, AT-02 pass |
| NFR-02 (transparency) | D-04, D-05 | All outputs auditable |
| NFR-04 (resilience) | D-04 | AT-05 pass |

---

## Decisions That Materially Changed Direction

| Decision | Before | After | Why It Mattered |
|----------|--------|-------|-----------------|
| **D-01** | "Build a price discovery app" | "Build a net-proceeds comparison tool" | E02 showed highest quote ≠ best outcome; shifted entire value proposition |
| **D-03** | "LLM can calculate and explain" | "Deterministic engine calculates; LLM only explains" | Prevented hallucinated numbers; separated correctness from communication |
| **D-04** | "Fill missing data with estimates" | "Flag missing/stale explicitly, no fabrication" | Aligned with case boundary + trust evidence (E04) |
| **D-05** | "Recommend best market" | "Present comparison, farmer decides" | Respected case boundary; maintained farmer agency |
| **D-06** | "Leverage existing KrishiVed components" | "Declare all New; prior work = knowledge only" | Honest disclosure; avoids misrepresentation risk |

---

## Unresolved Decisions (Post-Round 1)

| Decision ID | Question | Dependency | Target |
|-------------|----------|------------|--------|
| UD-01 | Hindi/local language support | U06 (accessibility needs), stakeholder confirmation | Build Round |
| UD-02 | Live mandi API integration | Access verification, coverage, freshness SLAs | Build Round |
| UD-03 | Harvest-timing recommendation viability | U04 (quality loss data), crop-specific research | Future |
| UD-04 | SMS/IVR/WhatsApp offline modes | U06, partner integration | Future |
| UD-05 | Collective selling / group transport features | Stakeholder demand, logistics partnerships | Future |