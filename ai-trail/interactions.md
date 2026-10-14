# AI Interaction Log

**Nature of this log**: These entries document analytical interactions that occurred during the hackathon working session on 2026-10-14. Each interaction represents a real conversation with an AI system (Claude, opencode CLI) that materially shaped team thinking. Timestamps indicate relative ordering within the session, not exact clock times.

**Session context**: The team used AI as a thinking partner throughout problem framing, evidence classification, solution design, and quality auditing. Each interaction below records the genuine exchange, the team's independent verification, and the resulting decision.

---

## Interaction AI-01: Problem Framing Exploration

| Field | Detail |
|-------|--------|
| **Session reference** | Working session 2026-10-14, interaction 1 of 8 |
| **Tool/Model** | Claude (opencode CLI) |
| **Question/Prompt** | "Compare three plausible problem framings for the agriculture case: (A) price discovery, (B) net proceeds comparison, (C) harvest timing. Use only the case evidence provided. Identify trade-offs, assumptions, and which is most defensible for a Version 1 scope." |
| **Key Response** | AI proposed the three framings with trade-offs. Highlighted that E01+E02 directly support net-proceeds comparison, while harvest timing (C) requires crop-quality models not in evidence. Suggested decision matrix criteria. |
| **Verification** | Team independently mapped case evidence (E01–E06) to each framing. Confirmed E03/E04/E05/E06 are supporting context, not core to any single framing. |
| **Alternatives Considered** | Whether to combine B+C as "selling decision with timing"; rejected due to V1 feasibility. Whether price discovery (A) is sufficient; rejected per E02. |
| **Team Decision** | Accepted Framing B (Net Proceeds Comparison) as primary. Weather/connectivity/trust as supporting features. |
| **Rationale** | Strongest evidence alignment (E01+E02), feasible deterministic core, testable, extensible. |
| **Impact** | **Changed direction** — initial team bias was toward "price discovery app"; evidence shifted to net-proceeds comparison. |
| **Evidence Link** | This conversation; `docs/01-problem-framing.md` |

---

## Interaction AI-02: Evidence Classification

| Field | Detail |
|-------|--------|
| **Session reference** | Working session 2026-10-14, interaction 2 of 8 |
| **Tool/Model** | Claude (opencode CLI) |
| **Question/Prompt** | "Classify each case evidence item (E01–E06) as: simulated case evidence / externally verified / assumption / unknown. For each, state the design consequence for a net-proceeds comparison tool." |
| **Key Response** | AI produced classification table. Emphasized all numerical claims are simulated. Flagged U01–U06 as unresolved. |
| **Verification** | Team cross-checked each claim against case PDF text. Confirmed "simulated" label accuracy. |
| **Alternatives Considered** | Whether to seek external verification for transport costs; decided V1 uses user estimates with clear labeling. |
| **Team Decision** | Adopted AI's classification; added A01–A05 assumptions and U01–U06 unknowns explicitly. |
| **Rationale** | Transparency requirement: judges must see what is case evidence vs. team assumption. |
| **Impact** | **Validated approach** — evidence ledger structure confirmed. |
| **Evidence Link** | This conversation; `docs/02-evidence-ledger.md` |

---

## Interaction AI-03: Version 1 Boundary Definition

| Field | Detail |
|-------|--------|
| **Session reference** | Working session 2026-10-14, interaction 3 of 8 |
| **Tool/Model** | Claude (opencode CLI) |
| **Question/Prompt** | "Define a Version 1 boundary for a net-proceeds comparison tool. What must be included vs. explicitly excluded? Consider: deterministic vs AI calculations, live vs simulated data, languages, offline, harvest timing, collective selling." |
| **Key Response** | AI proposed V1 boundary table. Key insight: deterministic engine for calculations, AI only for explanations. Simulated data primary. English only. No harvest timing. No SMS/IVR. |
| **Verification** | Team reviewed against Round 1 requirements (feasible, testable, transparent). Confirmed all "excluded" items are explicitly documented as future work. |
| **Alternatives Considered** | Including Hindi in V1; deferred to future per team constraint. Including live API as must-have; changed to prototype-only. |
| **Team Decision** | Adopted boundary with minor edits (added explicit NFR for language reliability). |
| **Rationale** | Scope discipline: judges evaluate thinking quality, not feature breadth. |
| **Impact** | **Defined scope** — prevented feature creep in SRS. |
| **Evidence Link** | This conversation; `specs/01-solution-srs.md` (Section 2.5) |

---

## Interaction AI-04: Deterministic vs AI Responsibility Split

| Field | Detail |
|-------|--------|
| **Session reference** | Working session 2026-10-14, interaction 4 of 8 |
| **Tool/Model** | Claude (opencode CLI) |
| **Question/Prompt** | "For the net-proceeds tool, which computations must be deterministic (not LLM) and why? Define the AI/human boundary precisely." |
| **Key Response** | AI identified: arithmetic, ranking, timestamp checks, validation = deterministic. Explanation generation, translation = AI. Farmer = final decision. Provided model card template. |
| **Verification** | Team verified: LLM cannot reliably do arithmetic; explanations can hallucinate numbers. Boundary prevents "AI calculates net proceeds" anti-pattern. |
| **Alternatives Considered** | LLM for entire pipeline; rejected due to correctness risk. |
| **Team Decision** | Hard boundary: deterministic engine is separate module, tested independently. AI explanation layer receives only validated JSON. |
| **Rationale** | "An LLM can produce a plausible explanation even when underlying data is wrong" — separates correctness from communication. |
| **Impact** | **Architectural decision** — driven module separation in `specs/01-solution-srs.md` and `specs/02-ai-human-model.md`. |
| **Evidence Link** | This conversation; `specs/02-ai-human-model.md` |

---

## Interaction AI-05: Missing/Stale Data Handling

| Field | Detail |
|-------|--------|
| **Session reference** | Working session 2026-10-14, interaction 5 of 8 |
| **Tool/Model** | Claude (opencode CLI) |
| **Question/Prompt** | "How should the system handle: (a) missing market quote, (b) stale quote (>4h), (c) conflicting prices, (d) offline with cache? Must not fabricate data." |
| **Key Response** | AI proposed explicit handling rules: flag missing/stale, show all conflicts, offline banner with cache timestamp, no auto-resolution. Generated AT-03, AT-04, AT-05 test cases. |
| **Verification** | Team confirmed: case evidence E03 (connectivity) + E01 (quote freshness unknown) require this. No fabrication aligns with case boundary. |
| **Alternatives Considered** | Interpolate missing quotes from neighbors; rejected — invents data. Auto-pick freshest; rejected — hides conflict. |
| **Team Decision** | Adopted explicit flagging + farmer verification model. |
| **Rationale** | Trust (E04) requires transparency over convenience. |
| **Impact** | **FR-05, FR-09, AT-03/04/05 defined**. |
| **Evidence Link** | This conversation; `specs/01-solution-srs.md` (Section 2.4), `tests/acceptance-tests.md` |

---

## Interaction AI-06: Acceptance Test Adequacy

| Field | Detail |
|-------|--------|
| **Session reference** | Working session 2026-10-14, interaction 6 of 8 |
| **Tool/Model** | Claude (opencode CLI) |
| **Question/Prompt** | "Review the 6 acceptance tests. Are they sufficient for Round 1? Do they cover all must-have FRs? Are fixtures independently verifiable?" |
| **Key Response** | AI confirmed AT-01/02 cover FR-03/04, AT-03/04/05 cover FR-05/09, AT-06 covers FR-06/07. Noted AT-02 is critical (tests core insight: headline price ≠ net proceeds). Suggested adding explicit fixture for conflicting prices. |
| **Verification** | Team added AT-02 fixture showing tie-break on net proceeds. Confirmed all fixtures manually calculable. |
| **Alternatives Considered** | Adding performance tests; deferred — NFR targets not yet set. |
| **Team Decision** | Kept 6 tests; marked all "Not run" honestly. |
| **Rationale** | Round 1 requires test definitions; execution is Build Round. |
| **Impact** | **Test suite finalized**. |
| **Evidence Link** | This conversation; `tests/acceptance-tests.md` |

---

## Interaction AI-07: Existing Component Disclosure

| Field | Detail |
|-------|--------|
| **Session reference** | Working session 2026-10-14, interaction 7 of 8 |
| **Tool/Model** | Claude (opencode CLI) |
| **Question/Prompt** | "Our team has prior KrishiVed agriculture project work. For this hackathon submission (new repo, fresh start), how should we disclose existing components honestly?" |
| **Key Response** | AI advised: if no code/assets are actually reused in this submission, declare all as "New". Do not claim reuse for credibility. Disclose prior work only if it materially informed design (e.g., UI patterns, domain knowledge). |
| **Verification** | Team confirmed: this repo is empty; no components copied. Prior work informed domain understanding only. |
| **Alternatives Considered** | Claiming "existing React components reused"; rejected — not true. |
| **Team Decision** | All components = New. Prior KrishiVed noted as domain knowledge source only. |
| **Rationale** | Competition requires honest disclosure; misrepresentation risks disqualification. |
| **Impact** | `ai-trail/existing-components.md` reflects accurate status. |
| **Evidence Link** | This conversation; `ai-trail/existing-components.md` |

---

## Interaction AI-08: Final Submission Audit

| Field | Detail |
|-------|--------|
| **Session reference** | Working session 2026-10-14, interaction 8 of 8 |
| **Tool/Model** | Claude (opencode CLI) |
| **Question/Prompt** | "Review the complete submission package against the 91-item Round 1 checklist. Identify gaps, inconsistencies, unverified claims, fabricated evidence." |
| **Key Response** | AI identified: (1) all numerical claims need "simulated" labels, (2) decision index needs explicit links to interactions, (3) existing-component disclosure must match repo reality, (4) AI log timestamps were fabricated and needed honest references. |
| **Verification** | Team applied fixes: added "simulated" labels throughout, linked decision index to AI-01–AI-08, confirmed all components New, replaced fabricated timestamps with honest session references. |
| **Alternatives Considered** | Whether to remove AI log entirely; rejected — genuine analytical reasoning occurred and should be documented. |
| **Team Decision** | Applied all fixes before final assembly. Honest session references used instead of fabricated clock times. |
| **Rationale** | Quality gate: submission must withstand scrutiny; fabricated evidence risks disqualification. |
| **Impact** | **Final audit pass** — AI trail now honest and verifiable. |
| **Evidence Link** | This conversation; `SUBMISSION.md` |
