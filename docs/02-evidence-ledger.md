# Evidence Ledger

All numerical claims below are **simulated case evidence** from the competition case file (ps.pdf). They are NOT independently verified real-world statistics. Each entry is labeled accordingly.

---

## Case-Provided Scenario Evidence (E01–E06)

| Evidence ID | Claim | Source | Classification | Relevance to Selected Problem | Confidence | Limitation | Design Consequence |
|-------------|-------|--------|----------------|-------------------------------|------------|------------|-------------------|
| **E01** | Mandi quotes range ₹14–24/kg across 4 markets (same morning) | Case evidence pack | Simulated case evidence | Establishes price spread; net proceeds depend on costs, not just headline price | Medium — simulated, not verified real data | Whether quoted prices are actually obtainable by a specific farmer is uncertain | Must show quote source & timestamp; flag unobtainable prices; do not rank solely by headline price |
| **E02** | 90 km market may offer higher price, but transport & loading remove ₹4–6/kg of advantage | Case evidence pack | Simulated case evidence | Core justification for net-proceeds comparison over price discovery | Medium — simulated | How costs vary by volume, shared transport, and loading/unloading specifics unknown | Transport cost must be user-entered or estimated with clear labeling; sensitivity display desirable |
| **E03** | 2 of 5 villages report unreliable mobile data during parts of the day | Case evidence pack | Simulated case evidence | System must handle intermittent connectivity gracefully | Medium — simulated | Whether SMS, IVR, WhatsApp, offline-first, or human intermediaries are better is unknown | V1: cache quotes with timestamps, show stale-data warnings, keep calculations available offline; SMS/IVR = future |
| **E04** | Farmers trust local traders & neighbours more than unfamiliar app recommendations | Case evidence pack | Simulated case evidence | Explanations & local verification matter more than opaque recommendations | Medium — simulated | What evidence would change or supplement trusted routines is unknown | AI explanations must be transparent, show sources/assumptions, preserve farmer decision authority |
| **E05** | Forecasts indicate possible heavy rain within 36 hours | Case evidence pack | Simulated case evidence | Weather context relevant to selling decision (quality/price risk) | Medium — simulated | How local conditions differ from district-level forecasts unknown | Show weather with source, update time, geographic coverage label; no harvest-timing recommendation |
| **E06** | Farmers estimate 8–15% post-harvest loss in weeks of oversupply | Case evidence pack | Simulated case evidence | Perishability context; potential value of better timing | Medium — simulated | Which part of loss is avoidable through better decisions unknown | Flag as contextual info; do not claim system reduces losses without evidence |

---

## Team Assumptions (A01–A05)

| Assumption ID | Assumption | Basis | Validation Needed |
|---------------|------------|-------|-------------------|
| **A01** | Farmers can estimate or obtain transport/loading costs for their specific volume & route | Case implies cost awareness (E02) | Survey or field validation; V1 uses user-entered estimates with clear labeling |
| **A02** | Mandi quotes have identifiable source & timestamp (even if simulated) | Required for freshness assessment | Prototype integration must preserve provenance metadata |
| **A03** | Net proceeds comparison (price minus selling costs) is a meaningful decision criterion for target farmers | Core problem framing selection | Usability testing with farmer proxies; V1 assumes yes |
| **A04** | English-only UI is acceptable for V1 (Hindi = future) | Team decision per project constraints | Confirm with stakeholders; V1 documents this boundary |
| **A05** | Simulated/test dataset is sufficient for Round 1 demonstration | No live API access verified yet | If live API becomes available, integrate as prototype layer with clear labeling |

---

## Unresolved Questions (U01–U06)

| Question ID | Question | Impact if Unresolved | V1 Treatment |
|-------------|----------|---------------------|--------------|
| **U01** | Are quoted prices actually obtainable by the farmer (vs. reference/indicative)? | Overstated net proceeds if unobtainable | Label quotes as "quoted/reference"; farmer must verify |
| **U02** | How do transport costs vary by quantity, distance, shared transport? | Inaccurate net proceeds | User-entered estimate; sensitivity note in explanation |
| **U03** | How fresh must a quote be to remain useful? | Stale data misleads | Configurable freshness threshold (default: 4 hrs); explicit warning |
| **U04** | What is expected quality loss from delaying sale for a specific crop? | Cannot support harvest-timing recommendation | Explicitly excluded from V1; flagged as future research |
| **U05** | How reliably does district forecast represent farmer's local conditions? | Weather-based advice may be wrong | Show forecast with coverage label; no decision automation |
| **U06** | Which farmers need offline, SMS, voice, or intermediary-assisted access? | Exclusion risk | V1: cached data + warnings; accessibility modes = future |

---

## External Sources (Placeholder — to be populated if any used)

| Source ID | Title | Publisher | URL | Access Date | Claim Supported | Verification Status |
|-----------|-------|-----------|-----|-------------|-----------------|---------------------|
| — | — | — | — | — | — | No external sources used in V1 evidence base. All claims from case file (simulated). |

**Note**: Per case file instruction, case-provided numerical entries are competition materials and should not be presented as verified real-world statistics about a named organisation or community.