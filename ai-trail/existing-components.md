# Existing Component Disclosure

## Declaration

**This submission (KrishiVed Market Decision Support) is built from scratch in a fresh repository.** No code, components, datasets, or integrations from prior projects (including KrishiVed) have been copied, adapted, or reused in this submission.

All components listed below are **New** — created specifically for this Round 1 submission.

---

## Component Register

| Component | Status | Description | Evidence |
|-----------|--------|-------------|----------|
| **Farmer Interface (React)** | New | Mobile-first input form, comparison table, warnings, weather panel — designed for this submission | `specs/01-solution-srs.md` UI requirements; no prior code in repo |
| **Mandi-Price Integration** | New (Simulated) | `tests/fixtures/market_data.json` — labelled simulated dataset; optional prototype API adapter scaffold | `specs/01-solution-srs.md` Section 4; fixtures version-controlled |
| **Weather Display** | New | Forecast panel showing condition, probability, source, timestamp, coverage label | `specs/01-solution-srs.md` FR-07; no prior implementation |
| **Multilingual Interface** | New (English only) | English UI for V1; Hindi marked as Future (FR-08) | `specs/01-solution-srs.md` V1 boundary |
| **Net-Proceeds Calculation Module** | New | Pure deterministic functions (`calculate_net_proceeds`), unit-testable, zero dependencies | `specs/01-solution-srs.md` Section 1.3; AT-01, AT-02 fixtures |
| **Market Comparison & Decision Logic** | New | Ranking by total net proceeds, data-freshness evaluation, conflict detection | `specs/01-solution-srs.md` FR-04, FR-05 |
| **Offline / Cached Behaviour** | New | Service worker cache strategy, offline banner, freshness threshold (configurable, default 4h) | `specs/01-solution-srs.md` FR-09, AT-05 |
| **AI Explanation Layer** | New | Template-based + LLM-assisted explanation generator (English V1); receives only validated engine output | `specs/02-ai-human-model.md`; prompt version-controlled |
| **Simulated Dataset** | New | `tests/fixtures/` — 6 test fixtures for AT-01 through AT-06, all labelled `simulated` | `tests/acceptance-tests.md` fixtures section |
| **Acceptance Test Suite** | New | 6 test definitions with pass criteria, fixtures, execution tracker | `tests/acceptance-tests.md` |
| **Documentation Package** | New | All `.md` files in `docs/`, `specs/`, `tests/`, `ai-trail/`, `references/` | This repository |

---

## Prior Work Acknowledgement

| Prior Project | Relationship to This Submission |
|---------------|---------------------------------|
| **KrishiVed** (team's prior agriculture AI project) | **Domain knowledge only** — informed understanding of mandi systems, farmer pain points, weather context, trust dynamics. **No code, UI components, APIs, or datasets reused.** |
| **AgriNova** (if applicable) | Not applicable — team confirms no AgriNova assets used. |

**Team Confirmation**: We have verified the repository state (`git log`, `ls -la`) and confirm zero pre-existing assets in this submission workspace.

---

## Mock / Simulated Integrations (Explicitly Declared)

| Integration | Status | Labeling in Submission |
|-------------|--------|------------------------|
| Mandi price API (eNAM, state APIs) | **Not integrated** — prototype scaffold only | `specs/01-solution-srs.md`: "Prototype tier — access not verified" |
| Weather API (IMD, OpenWeatherMap) | **Not integrated** — data structure defined only | `specs/01-solution-srs.md`: "Weather integration: forecast API with timestamp + coverage label" |
| Offline cache (Service Worker) | **Designed, not implemented** | `specs/01-solution-srs.md`: FR-09, AT-05 define behavior |
| AI Explanation (LLM) | **Prompt-designed, not deployed** | `specs/02-ai-human-model.md`: Model card with fallback template |

---

## Compliance Statement

This disclosure satisfies the Round 1 requirement: *"any prototype, dataset, mock integration or pre-existing component to be clearly declared."*

All claims in the submission documents (`docs/`, `specs/`, `tests/`) are consistent with this register. No component is presented as implemented, tested, or deployed unless explicitly marked with evidence (e.g., test fixtures exist and are version-controlled).

**Signed**: Team KrishiVed Market Decision Support
**Date**: 2026-10-14