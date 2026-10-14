# Evaluation Plan

Beyond acceptance tests, this plan assesses whether the design is useful, trustworthy, and understandable.

---

## Evaluation Dimensions

| Dimension | Method | Success Indicator | Status |
|-----------|--------|-------------------|--------|
| **Calculation correctness** | Manual verification of AT-01, AT-02 against independent calculator | 100% match | Not run |
| **Missing/stale data handling** | Execute AT-03, AT-04, AT-05; inspect UI warnings | All flags visible, no fabrication | Not run |
| **Explanation ↔ calculation consistency** | Compare AT-01/02 explanation text numbers to engine output | Zero discrepancies | Not run |
| **Understandability** | Proxy user (non-technical) walks through AT-02 scenario; identifies higher-net market and key assumption | >80% success rate | Not run |
| **Assumption visibility** | Check assumption panel shows: transport estimated, quote freshness, weather coverage | All present | Not run |
| **Connectivity loss behavior** | Simulate offline (DevTools); verify AT-05 behavior | Banner shown, calculations work, no false freshness claim | Not run |

---

## Evaluation Protocol

### Correctness Check (AT-01, AT-02)
1. Load fixture into deterministic engine
2. Record engine output (net realization, total proceeds, ranking)
3. Independently calculate expected values using manual arithmetic
4. Compare — discrepancy > ₹0.01 = FAIL

### Resilience Check (AT-03, AT-04, AT-05)
1. Load fixture with missing/stale/offline conditions
2. Inspect UI output for: data_freshness flags, warnings, offline banner
3. Verify no fabricated data appears in any field
4. Verify calculations still work on available data

### Explanation Consistency Check (AT-06)
1. Run AT-01/AT-02 with explanation layer enabled
2. Extract all numerical claims from explanation text
3. Compare each to deterministic engine output
4. Any mismatch in numbers, units, or caveats = FAIL

### Understandability Check
1. Recruit 1-2 proxy users (non-technical, familiar with mandi context)
2. Present AT-02 scenario (Mandi A ₹22/kg vs Mandi B ₹24/kg)
3. Ask: "Which market gives you more money after costs?" and "Why?"
4. Success if user identifies Mandi A and mentions transport costs

---

## Limitations of This Evaluation

- **Proxy users only** — not actual target farmers; results indicate direction, not field readiness
- **Simulated data** — does not test real-world API reliability, quote freshness, or connectivity patterns
- **No outcome measurement** — cannot claim income improvement, waste reduction, or decision quality without field study
- **English only** — does not evaluate Hindi comprehension or multilingual consistency

---

## Post-Round 1 Evaluation (Build Round)

| Phase | Method | Target |
|-------|--------|--------|
| **Field proxy testing** | 3-5 farmers walkthrough with real quotes (manual entry) | Identify top 3 misunderstandings |
| **API reliability** | 7-day monitoring of live mandi data availability, freshness, conflicts | Quantify data gap frequency |
| **Hindi comprehension** | Bilingual users compare English vs Hindi explanations | >90% numerical accuracy preserved |
| **Offline resilience** | 2G/3G simulation with intermittent connectivity | No false freshness claims |
| **Decision support value** | Before/after survey: confidence in selling decision | Measurable confidence increase |
