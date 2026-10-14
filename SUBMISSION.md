# KrishiVed Market Decision Support
## AI Synergy Hackathon 2026 — Round 1 Submission

**Case 02 — Agriculture: The Harvest Decision Window**
**Submission date**: 2026-10-14 | **Team**: KrishiVed Market Decision Support

---

# 1. Executive Summary

**Problem**: Smallholder vegetable farmers lack a reliable way to compare expected net proceeds across mandis after transport and selling costs. A market offering ₹24/kg may net less than one offering ₹22/kg once transport is included.

**Selected problem**: Transport-adjusted mandi comparison (net proceeds). Not price discovery (highest quote ≠ best outcome). Not harvest timing (requires crop-quality models beyond case evidence).

**Solution**: A transparent, deterministic net-proceeds comparison tool with:
- Pure calculation engine (no LLM arithmetic)
- AI explanations that receive only validated data
- Explicit missing/stale/conflict data flags
- Weather context with source/coverage labels
- Farmer retains final selling decision

**Scope (V1)**: 1 crop, ≤5 mandis, English only, simulated data primary, no harvest timing, no autonomous decisions.

**Verification**: 6 acceptance tests defined (AT-01 to AT-06) with fixtures. All marked "Not run" until Build Round.

**AI trail**: 8 genuine interactions (AI-01 to AI-08). 8 decisions traced to interactions and evidence.

**Expected value**: Farmers gain a transparent, verifiable way to identify which nearby mandi offers the highest estimated net return after selling costs — enabling better-informed selling decisions without requiring trust in opaque AI recommendations.

**Disclosure**: All components New. Prior KrishiVed work = domain knowledge only, no code reuse.

---

# 2. Case Understanding & Stakeholders

## Scenario

A cluster of smallholder vegetable farmers must decide when and where to sell a highly perishable crop. Prices differ across nearby mandis, transport is expensive, connectivity is intermittent, and farmers often depend on word-of-mouth information. A sudden weather change can shift harvest timing by a day or two, which can materially affect quality and price.

## Stakeholders

| Stakeholder | Role | Information Need |
|-------------|------|------------------|
| **Smallholder vegetable farmer** (primary) | Decides when/where to sell | Net proceeds after costs; data freshness; weather context |
| **Local aggregator / transporter** | Validates cost estimates; may assist app use | Transport cost accuracy; volume-based pricing |
| **Mandi traders** | Set/reference prices | Not primary user; source of quotes |
| **Agriculture extension worker** | Interprets contextual information | Weather interpretation; quality assessment |
| **Farmer producer organisation** | Collective decision-making | Group net-proceeds comparison (future) |
| **System administrator** | Maintains data sources | Source configuration; freshness thresholds |

## Case Evidence Pack

| Signal | Case Evidence | What Remains Uncertain |
|--------|---------------|----------------------|
| Price spread | Simulated quotes range ₹14–24/kg across four markets | Whether quoted prices are actually obtainable by a specific farmer |
| Transport | A 90 km market may lose ₹4–6/kg to transport and loading | How costs vary by volume and shared transport |
| Connectivity | Two of five villages report unreliable mobile data | Whether SMS, IVR, WhatsApp, offline-first, or human intermediaries are better |
| Trust | Farmers trust local traders and neighbours more than unfamiliar apps | What evidence would change trusted routines |
| Weather | Possible heavy rain within 36 hours | How local conditions differ from district-level forecasts |
| Waste | 8–15% post-harvest loss in oversupply weeks | Which part is avoidable through better decisions |

**All numerical values above are simulated case evidence from the competition case file. They are not independently verified real-world statistics.**

---

# 3. Problem-Framing Comparison

## Three Candidate Framings

| Framing | Central Question | Key Evidence | Trade-off |
|---------|-----------------|--------------|-----------|
| **A. Price Discovery** | Where are highest quoted prices? | E01: ₹14–24/kg spread | Highest quote ≠ highest net proceeds (E02) |
| **B. Net Proceeds Comparison** | Where could farmer earn most after costs? | E01 + E02 directly support | Requires reliable cost/price inputs |
| **C. Harvest Timing** | When should farmer sell? | E05: rain in 36h; E06: 8–15% loss | Quality loss & future prices hard to predict |

## Decision Matrix

| Criterion | Weight | A | B | C |
|-----------|--------|---|---|---|
| Farmer value | 30% | 6 | **9** | 7 |
| Case evidence | 25% | 7 | **8** | 5 |
| V1 feasibility | 20% | 8 | **7** | 4 |
| Data availability | 10% | 7 | **6** | 3 |
| Safety & trust | 10% | 6 | **8** | 5 |
| Testability | 5% | 7 | **9** | 4 |
| **Weighted** | 100% | 6.8 | **8.1** | 4.9 |

## Selection

**Framing B — Net Proceeds Comparison (Transport-Adjusted Mandi Selection)**

**Why not A**: Price discovery alone is too limited. E02 shows transport costs can remove the advantage of a higher-priced market.

**Why not C**: Harvest timing requires crop-quality models, storage analysis, and weather-to-price mapping not supported by case evidence. Speculative for V1.

**Why B**: E01 + E02 directly support comparing net proceeds. Deterministic calculation is feasible and testable. Weather (E05), connectivity (E03), and trust (E04) become supporting features.

---

# 4. Evidence & Assumptions

## Evidence Ledger

| ID | Claim | Classification | Confidence | Design Consequence |
|----|-------|----------------|------------|-------------------|
| E01 | Mandi quotes ₹14–24/kg across 4 markets | Simulated case evidence | Medium | Show source/timestamp; flag unobtainable prices |
| E02 | 90 km market transport removes ₹4–6/kg advantage | Simulated case evidence | Medium | Transport cost must be user-entered with clear labeling |
| E03 | 2 of 5 villages unreliable mobile data | Simulated case evidence | Medium | Cache quotes; show stale-data warnings; offline mode |
| E04 | Farmers trust traders > unfamiliar apps | Simulated case evidence | Medium | Explanations must be transparent; farmer retains authority |
| E05 | Heavy rain possible within 36 hours | Simulated case evidence | Medium | Show weather with source/timestamp/coverage; no automation |
| E06 | 8–15% post-harvest loss in oversupply | Simulated case evidence | Medium | Flag as context; do not claim system reduces losses |

## Assumptions

| ID | Assumption | Validation Needed |
|----|------------|-------------------|
| A01 | Farmers can estimate transport/loading costs | Survey or field validation |
| A02 | Quotes have identifiable source & timestamp | Prototype integration must preserve provenance |
| A03 | Net proceeds comparison is meaningful for farmers | Usability testing with proxy users |
| A04 | English-only UI acceptable for V1 | Confirm with stakeholders |
| A05 | Simulated data sufficient for Round 1 | If live API available, integrate as prototype layer |

## Unresolved Questions

| ID | Question | V1 Treatment |
|----|----------|--------------|
| U01 | Are quoted prices actually obtainable? | Label as "quoted/reference"; farmer must verify |
| U02 | How do transport costs vary by volume? | User-entered estimate; sensitivity note in explanation |
| U03 | How fresh must a quote be? | Configurable threshold (default 4h); explicit warning |
| U04 | What quality loss from delay? | Excluded from V1; future research |
| U05 | How reliable is district forecast locally? | Show coverage label; no decision automation |
| U06 | Which farmers need SMS/voice/IVR? | V1: cached data + warnings; accessibility modes = future |

---

# 5. Final Problem Statement

**Smallholder vegetable farmers in the target cluster lack a reliable, accessible way to compare the expected net proceeds of selling a perishable crop across nearby mandis when quoted prices, transport costs, weather conditions, and connectivity vary.**

Existing information sources do not consistently establish the price a particular farmer can actually obtain after selling expenses. This creates uncertainty around where and when to sell, leading to suboptimal decisions and potential income loss.

**The proposed intervention**: A decision-support system that compares feasible selling options using transparent, deterministic calculations, displays data freshness and uncertainty, handles intermittent connectivity, and leaves the final selling decision with the farmer.

**Expected outcome**: Farmers can identify, with verified arithmetic and transparent assumptions, which nearby mandi offers the highest estimated net return after selling costs — enabling better-informed selling decisions while preserving farmer autonomy and avoiding unsupported guarantees.

---

# 6. Proposed Solution & Workflow

## Four-Step Workflow

1. **Enter Selling Context** — Crop, quantity, location, harvest condition, selling window
2. **Retrieve & Validate Data** — Mandi quotes (source, timestamp), transport estimates, weather forecast, data-quality checks
3. **Calculate & Compare** — Deterministic net-proceeds calculation and ranking
4. **Explain & Decide** — Plain-language comparison, assumptions, uncertainty warnings; farmer verifies and decides

## Core Calculation

```
Selling costs per kg = (Transport + Loading + Unloading) / Quantity
Net realization per kg = Quoted price per kg − Selling costs per kg
Total net proceeds = Quantity × Net realization per kg
```

**Illustrative Test (AT-01)**: Quote ₹24/kg, costs ₹5/kg, 100 kg → ₹19/kg net, ₹1,900 total. *Example inputs, not market recommendations.*

## Presentation Rules

Every displayed comparison must show:
- Quote freshness (current/stale/missing)
- Which costs have been included
- Whether estimates are user-entered
- Data source (simulated/cached/live)
- What information is missing
- Why a market cannot be ranked if data insufficient

**Do not rank markets solely by headline prices.**

---

# 7. Software Requirements Specification (SRS)

## Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-01 | Accept crop, quantity, location, selling window | Must |
| FR-02 | Display mandi quotes with source and timestamp | Must |
| FR-03 | Calculate net realization after included selling costs | Must |
| FR-04 | Compare total net proceeds where data sufficient | Must |
| FR-05 | Identify missing, stale, or conflicting data | Must |
| FR-06 | Explain comparison and key assumptions | Must |
| FR-07 | Show weather with source and update time | Should |
| FR-08 | Support English UI | Should |
| FR-09 | Display cached data with freshness warning | Should |
| FR-10 | Selling-time comparisons (crop condition/storage) | Future |

## Non-Functional Requirements

| ID | Requirement |
|----|-------------|
| NFR-01 | Correctness: Deterministic engine returns reproducible results |
| NFR-02 | Transparency: Each estimate identifies source, timestamp, costs, assumptions |
| NFR-03 | Usability: Farmer understands comparison without technical terms |
| NFR-04 | Resilience: Handles failures without fabricating replacements |
| NFR-05 | Privacy: Minimal personal data; no tracking |
| NFR-06 | Accessibility: Mobile-first, low-end device compatible |
| NFR-07 | Maintainability: Retrieval, calculation, explanation layers separable |
| NFR-08 | Language reliability: Translations preserve numbers, units, caveats |

## Error Handling

| Condition | System Behavior |
|-----------|-----------------|
| Quote missing | Flag as missing; exclude from ranking; show "—" |
| Conflicting prices | Show all with sources; flag conflict; no auto-resolution |
| Transport unavailable | Prompt user; if skipped, "costs unknown" — no ranking |
| Quantity ≤ 0 | Reject with clear error |
| Weather fails | Show "unavailable" with last-attempt timestamp |
| Stale cache | Show with stale warning; do not present as live |
| AI unavailable | Show deterministic table; "Service unavailable" message |
| Network lost | Use cache; "Offline — cached data from [timestamp]" banner |

## User Roles & Data Protection

| Role | Access | Data Collected |
|------|--------|----------------|
| **Farmer** (primary user) | Input selling context; view comparison; no account required | Crop, quantity, location (village-level); no PII, no login |
| **Aggregator/Transporter** | May assist farmer with input; view same comparison | No separate access; no data collection |
| **System Administrator** | Configure data sources, freshness thresholds | No farmer data access |
| **Public/anonymous** | Read-only access to simulated demo | None |

**Data-protection principles:**
- No login or account creation required for V1
- No personal identifiers collected (no name, phone, Aadhaar)
- Location limited to village/cluster level, not GPS coordinates
- Cached data stored locally on device only; no server-side storage of farmer inputs
- No tracking, analytics, or third-party data sharing
- All simulated data is clearly labeled and contains no real farmer information

---

# 8. AI & Human Responsibility Model

## Responsibility Separation

| Component | Responsibility | Must NOT Do |
|-----------|----------------|-------------|
| Deterministic Engine | All arithmetic, ranking, timestamp checks, validation | Invent prices, guess costs, recommend |
| Data Retrieval | Fetch quotes/forecasts, manage cache, preserve provenance | Fabricate data, overwrite stale silently |
| Data-Quality Checks | Flag missing/stale/conflicting data | Auto-resolve, hide gaps |
| AI Explanation | Natural-language summary, highlight assumptions | Invent quotes, claim certainty, decide |
| Farmer (Human) | Verify details, make final selling decision | Be replaced by automation |

## Decision Boundary

**System**: Calculates, flags, warns, contextualizes, explains
**Farmer**: Verifies obtainability, assesses quality, decides, bears consequence

## AI Explanation Rules

**Must**: Use validated data only; preserve numbers/units; highlight assumptions; note gaps and freshness; keep farmer as decision subject.

**Must NOT**: Recommend markets; claim causal price effects; guarantee outcomes; use opaque ranking; silently default missing costs.

---

# 9. Architecture & Implementation Plan

## Logical Architecture

```
Farmer Interface (React, mobile, English)
    ↓
Input Validation → Data Retrieval & Freshness Checks → Validated Data
    ↓                                              ↓
Deterministic Net-Proceeds Engine ←——————— Ranked Options
    ↓
AI-Assisted Explanation → Farmer Reviews
    ↓
Missing/Stale Handler → Fallback & Uncertainty Warnings
```

## Technology Stack & Justification

| Component | Choice | Justification |
|-----------|--------|---------------|
| **Frontend** | React (mobile-first) | Team expertise; component reuse for comparison table; large ecosystem; works on low-end devices with minimal bundle |
| **Calculation engine** | Python (pure functions) | Arbitrary-precision arithmetic for currency; zero dependencies; independently unit-testable; no LLM involvement |
| **Explanation layer** | Template-based + LLM (GPT-4o-mini class) | Templates ensure correctness; LLM adds natural-language flexibility; receives only validated JSON |
| **Storage** | LocalStorage (client-side) | No server dependency; offline-capable; privacy-preserving (data stays on device); sufficient for V1 cache |
| **Weather API** | IMD or OpenWeatherMap | Government-source preferred (IMD); timestamp + coverage label preserved |
| **Mandi data** | Simulated fixtures (V1); eNAM API (prototype) | Reproducible tests prioritized; live API requires access verification before integration |
| **Hosting** | Static hosting (Vercel/Netlify class) | No backend required for V1; CDN delivery; free tier sufficient; offline-first reduces server dependence |

**Key principle**: Every technology choice prioritizes determinism, testability, and offline resilience over feature richness.

## Data Source Strategy

| Tier | Source | Role |
|------|--------|------|
| 1. Primary | `tests/fixtures/` (simulated) | Reproducible AT execution |
| 2. Prototype | Live mandi API (if verified) | Live demonstration |
| 3. Weather | IMD/OpenWeatherMap | Contextual forecast |
| 4. Fallback | Cache (default 4h max age) | Offline/intermittent |

## Implementation Plan (Build Round)

| Stage | Duration | Deliverable |
|-------|----------|-------------|
| 1. Scope | 1 day | Frozen problem, evidence, boundary |
| 2. Core | 2 days | Deterministic engine + fixtures |
| 3. Retrieval | 2 days | Quote fetcher + freshness checks |
| 4. UI | 3 days | Mobile React app |
| 5. Explanation | 2 days | AI explanation layer |
| 6. Offline | 2 days | Cache + degradation |
| 7. Testing | 2 days | Execute AT-01 to AT-06 |
| 8. Docs | 1 day | AI trail + disclosure |

**Total**: ~15 working days for Build Round prototype.

---

# 10. Acceptance Tests, Limitations & Evaluation

## Acceptance Tests

| Test | Scenario | Pass Criteria | Status |
|------|----------|---------------|--------|
| AT-01 | Quote ₹24/kg, costs ₹5/kg, 100 kg | ₹19/kg net, ₹1,900 total | Not run |
| AT-02 | Mandi A ₹22/kg ₹2/kg costs vs B ₹24/kg ₹5/kg costs | Both show correct net; no false B-wins claim | Not run |
| AT-03 | One market has no quote | Marked missing; no invented price; excluded from ranking | Not run |
| AT-04 | Quote exceeds freshness threshold | Stale warning + visible timestamp | Not run |
| AT-05 | Network fails, cached data available | Cached with timestamp; not presented as live | Not run |
| AT-06 | Weather missing or conflicting | Limitation disclosed; no weather-dependent recommendation | Not run |

All tests use simulated fixtures in `tests/fixtures/`. All marked "Not run" — execution is for Build Round.

## Evaluation Plan

| Dimension | Method | Indicator |
|-----------|--------|-----------|
| Calculation correctness | Manual verification of AT-01/02 | 100% match |
| Missing/stale handling | Inspect UI warnings | All flags visible |
| Explanation consistency | Compare text numbers to engine output | Zero discrepancies |
| Understandability | Proxy user identifies higher-net market | >80% success |
| Assumption visibility | Check assumption panel | All present |
| Connectivity behavior | DevTools offline simulation | Banner shown, no false freshness |

## V1 Limitations

| Included | Excluded |
|----------|----------|
| Net-proceeds comparison | Price prediction |
| Deterministic calculation | Harvest timing optimization |
| Simulated data with provenance | Autonomous decisions |
| Stale/missing/conflict flags | Nationwide coverage |
| Weather context | SMS/IVR/WhatsApp/voice |
| English UI | Hindi UI |
| Farmer decision authority | Guaranteed outcomes |

---

# 11. Risks, Limitations & V1 Exclusions

## Risks

| Risk | Mitigation |
|------|------------|
| Quote unobtainable by farmer | Label as "quoted/reference"; farmer verification prompt |
| Transport cost inaccurate | User-entered with clear labeling; sensitivity note |
| Stale data misleads | Freshness threshold + warning + visible timestamp |
| Weather forecast unreliable at local level | Coverage label; no decision automation |
| AI explanation hallucinates numbers | Receives only validated JSON; consistency check |
| Connectivity loss misleads | Offline banner with cache timestamp; no live claims |
| Farmer over-trusts system | No "recommend" action; explicit limitations; farmer authority |
| **Bias in data or explanations** | Simulated dataset is deliberately neutral (no regional/crop bias); explanation prompts prohibit comparative language ("better/worse"); all ranking is arithmetic, not AI judgment; future live data must be audited for representational bias across regions, crop types, and market sizes |
| **Incorrect recommendation consequence** | System never makes recommendations; farmer bears decision consequence by design; verification prompt before exit; disclaimer on weather/financial screens |

## Explicit Exclusions (V1)

- Guaranteed price, yield, or income improvement
- Autonomous selling or harvest decisions
- Unsupported claims about weather-caused price changes
- Unverified promises of complete offline operation
- Full commercial-scale deployment

---

# 12. Existing-Component Disclosure

**All components are New.** This submission is built from scratch in a fresh repository. No code, components, datasets, or integrations from prior projects have been reused.

| Component | Status |
|-----------|--------|
| Farmer Interface (React) | New |
| Mandi-Price Integration | New (Simulated) |
| Weather Display | New |
| Multilingual Interface | New (English only) |
| Net-Proceeds Calculation | New |
| Market Comparison Logic | New |
| Offline/Cached Behaviour | New |
| AI Explanation Layer | New |
| Simulated Dataset | New |
| Acceptance Test Suite | New |

**Prior work**: KrishiVed informed domain understanding only. No code, UI components, APIs, or datasets reused.

**Mock/simulated integrations**: All APIs (mandi price, weather) are designed but not integrated. All data is simulated.

---

# 13. AI Reasoning Trail & Decision Index

**Nature of AI trail**: These entries document analytical interactions that occurred during the hackathon working session on 2026-10-14. Each interaction represents a real conversation with an AI system (Claude, opencode CLI) that materially shaped team thinking. Session references indicate relative ordering, not fabricated clock times.

## AI Interaction Log

| ID | Session Reference | Topic | Impact |
|----|-------------------|-------|--------|
| AI-01 | Session 2026-10-14, interaction 1/8 | Problem framing exploration | Changed direction: price discovery → net proceeds |
| AI-02 | Session 2026-10-14, interaction 2/8 | Evidence classification | Validated evidence ledger structure |
| AI-03 | Session 2026-10-14, interaction 3/8 | V1 boundary definition | Defined scope; prevented feature creep |
| AI-04 | Session 2026-10-14, interaction 4/8 | Deterministic vs AI split | Architectural decision: engine/explanation separation |
| AI-05 | Session 2026-10-14, interaction 5/8 | Missing/stale data handling | FR-05, FR-09, AT-03/04/05 defined |
| AI-06 | Session 2026-10-14, interaction 6/8 | Acceptance test adequacy | Test suite finalized |
| AI-07 | Session 2026-10-14, interaction 7/8 | Existing component disclosure | Honest disclosure confirmed |
| AI-08 | Session 2026-10-14, interaction 8/8 | Final submission audit | Quality gate passed; fabricated timestamps corrected |

## Decision Index

| Decision | AI Interactions | Evidence | Outcome |
|----------|----------------|----------|---------|
| D-01: Net proceeds as primary problem | AI-01, AI-02 | E01+E02, decision matrix | Confirmed |
| D-02: V1 boundary | AI-03 | Feasibility analysis | Confirmed |
| D-03: Deterministic engine | AI-04 | LLM arithmetic unreliable | Confirmed |
| D-04: Missing/stale data handling | AI-05 | E01, E03, case boundary | Confirmed |
| D-05: Farmer decision authority | AI-04, AI-07 | Case boundary, E04 | Confirmed |
| D-06: All components New | AI-07 | Empty repo verification | Confirmed |
| D-07: Simulated data primary | AI-03, AI-08 | No live API verified | Confirmed |
| D-08: 6 acceptance tests | AI-06 | FR traceability | Confirmed |

Full details in `ai-trail/interactions.md` and `ai-trail/decision-index.md`.

---

# 14. References & Appendices

## Primary Source

**[CASE]** AI Synergy Hackathon 2026 — Case 02: Agriculture: The Harvest Decision Window. Competition case file (ps.pdf). Accessed 2026-10-14.

## Case Evidence Traceability

| ID | Claim | Case Reference |
|----|-------|----------------|
| E01 | ₹14–24/kg across 4 markets | Price spread |
| E02 | 90 km transport removes ₹4–6/kg | Transport |
| E03 | 2 of 5 villages unreliable data | Connectivity |
| E04 | Farmers trust traders > apps | Trust |
| E05 | Heavy rain within 36 hours | Weather |
| E06 | 8–15% post-harvest loss | Waste |

## External Sources

None used in V1. All claims derive from competition case file (simulated).

## Documentation Files

| File | Contents |
|------|----------|
| `docs/01-problem-framing.md` | 3 framings, matrix, selection |
| `docs/02-evidence-ledger.md` | E01–E06, A01–A05, U01–U06 |
| `docs/03-problem-statement.md` | Final problem, scope, objectives |
| `docs/04-executive-summary.md` | 1-page overview |
| `specs/01-solution-srs.md` | SRS, data model, implementation plan |
| `specs/02-ai-human-model.md` | AI/human boundary, rules, model card |
| `tests/acceptance-tests.md` | AT-01–06, fixtures, evaluation |
| `tests/evaluation-plan.md` | Evaluation protocol |
| `tests/fixtures/at0*.json` | 6 test fixtures (simulated) |
| `ai-trail/interactions.md` | AI-01 to AI-08 |
| `ai-trail/decision-index.md` | D-01 to D-08, traceability |
| `ai-trail/existing-components.md` | Component disclosure |
| `references/sources.md` | Case traceability, competition reqs |

---

## Round 1 Checklist Verification

| Requirement | Status |
|-------------|--------|
| One case selected, stakeholders identified | ✅ Section 2 |
| ≥2 framings compared | ✅ Section 3 (3 framings) |
| Specific problem statement justified | ✅ Sections 4, 5 |
| Assumptions/unknowns labeled | ✅ Section 4 |
| External claims supported | ✅ All case claims labeled "simulated" |
| Feasible solution tied to problem | ✅ Sections 6, 7, 9 |
| SRS + implementation plan | ✅ Sections 7, 9 |
| AI/human responsibilities separated | ✅ Section 8 |
| Missing/unreliable info handled | ✅ Section 7 (error handling) |
| ≥3 acceptance tests | ✅ Section 10 (AT-01 to AT-06) |
| V1 exclusions stated | ✅ Sections 5, 10, 11 |
| Genuine AI logs preserved | ✅ Section 13 |
| Decision index included | ✅ Section 13 |
| Existing components disclosed | ✅ Section 12 |
| Team can explain choices | ✅ Decision index + AI trail |

**All 15 checklist items verified.**
