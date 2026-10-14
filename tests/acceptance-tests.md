# Acceptance Tests (V1)

All tests use **simulated test fixtures** — not real market data. Tests verify requirement compliance, not agricultural outcomes.

---

## Test Fixtures

### Fixture: `tests/fixtures/at01-basic-calculation.json`
```json
{
  "crop": "tomato",
  "quantity_kg": 100,
  "markets": [
    {
      "market_id": "m1",
      "market_name": "Mandi A",
      "quoted_price_per_kg": 24,
      "quote_source": "simulated",
      "quote_timestamp": "2026-10-14T08:00:00Z",
      "transport_cost_total": 300,
      "loading_cost_total": 100,
      "unloading_cost_total": 100
    }
  ],
  "freshness_threshold_hours": 4
}
```
**Expected Calculation:**
- Selling costs/kg = (300+100+100)/100 = ₹5/kg
- Net realization = 24 - 5 = ₹19/kg
- Total net proceeds = 100 × 19 = ₹1,900

---

### Fixture: `tests/fixtures/at02-tie-on-net-proceeds.json`
```json
{
  "crop": "tomato",
  "quantity_kg": 100,
  "markets": [
    {
      "market_id": "m1",
      "market_name": "Mandi A",
      "quoted_price_per_kg": 22,
      "quote_source": "simulated",
      "quote_timestamp": "2026-10-14T08:00:00Z",
      "transport_cost_total": 100,
      "loading_cost_total": 50,
      "unloading_cost_total": 50
    },
    {
      "market_id": "m2",
      "market_name": "Mandi B",
      "quoted_price_per_kg": 24,
      "quote_source": "simulated",
      "quote_timestamp": "2026-10-14T08:00:00Z",
      "transport_cost_total": 300,
      "loading_cost_total": 100,
      "unloading_cost_total": 100
    }
  ],
  "freshness_threshold_hours": 4
}
```
**Expected Calculation:**
- Mandi A: costs = (100+50+50)/100 = ₹2/kg → net = 22-2 = ₹20/kg → total = ₹2,000
- Mandi B: costs = (300+100+100)/100 = ₹5/kg → net = 24-5 = ₹19/kg → total = ₹1,900
- **Ranking**: Mandi A (₹2,000) > Mandi B (₹1,900) despite lower headline price

---

### Fixture: `tests/fixtures/at03-missing-quote.json`
```json
{
  "crop": "tomato",
  "quantity_kg": 100,
  "markets": [
    {
      "market_id": "m1",
      "market_name": "Mandi A",
      "quoted_price_per_kg": 22,
      "quote_source": "simulated",
      "quote_timestamp": "2026-10-14T08:00:00Z",
      "transport_cost_total": 100,
      "loading_cost_total": 50,
      "unloading_cost_total": 50
    },
    {
      "market_id": "m2",
      "market_name": "Mandi B",
      "quoted_price_per_kg": null,
      "quote_source": "simulated",
      "quote_timestamp": null,
      "transport_cost_total": 300,
      "loading_cost_total": 100,
      "unloading_cost_total": 100
    }
  ],
  "freshness_threshold_hours": 4
}
```

---

### Fixture: `tests/fixtures/at04-stale-quote.json`
```json
{
  "crop": "tomato",
  "quantity_kg": 100,
  "markets": [
    {
      "market_id": "m1",
      "market_name": "Mandi A",
      "quoted_price_per_kg": 22,
      "quote_source": "simulated",
      "quote_timestamp": "2026-10-14T02:00:00Z",
      "transport_cost_total": 100,
      "loading_cost_total": 50,
      "unloading_cost_total": 50
    }
  ],
  "freshness_threshold_hours": 4
}
```
*Quote is 6 hours old (threshold = 4h) → stale*

---

### Fixture: `tests/fixtures/at05-offline-cached.json`
```json
{
  "crop": "tomato",
  "quantity_kg": 100,
  "markets": [
    {
      "market_id": "m1",
      "market_name": "Mandi A",
      "quoted_price_per_kg": 22,
      "quote_source": "cached",
      "quote_timestamp": "2026-10-14T06:00:00Z",
      "transport_cost_total": 100,
      "loading_cost_total": 50,
      "unloading_cost_total": 50
    }
  ],
  "freshness_threshold_hours": 4,
  "connectivity": "offline"
}
```

---

### Fixture: `tests/fixtures/at06-weather-missing.json`
```json
{
  "crop": "tomato",
  "quantity_kg": 100,
  "markets": [
    {
      "market_id": "m1",
      "market_name": "Mandi A",
      "quoted_price_per_kg": 22,
      "quote_source": "simulated",
      "quote_timestamp": "2026-10-14T08:00:00Z",
      "transport_cost_total": 100,
      "loading_cost_total": 50,
      "unloading_cost_total": 50
    }
  ],
  "freshness_threshold_hours": 4,
  "weather": null
}
```

---

## Acceptance Test Definitions

| Test ID | Requirement | Scenario | Pass Criteria | Status |
|---------|-------------|----------|---------------|--------|
| **AT-01** | FR-03, FR-04 | Basic net-proceeds calculation (fixture at01) | Engine returns: `net_realization_per_kg: 19`, `total_net_proceeds: 1900` | Not run |
| **AT-02** | FR-03, FR-04 | Higher headline price ≠ higher net proceeds (fixture at02) | Mandi A ranked above Mandi B; both show ₹20/kg and ₹19/kg net respectively; no claim that Mandi B wins | Not run |
| **AT-03** | FR-02, FR-05 | Missing quote (fixture at03) | Mandi B shows `data_freshness: "missing"`; excluded from ranking; no invented price; table shows "—" for quote | Not run |
| **AT-04** | FR-02, FR-05 | Stale quote (fixture at04) | Mandi A shows `data_freshness: "stale"`; warning "Quote is 6 hours old (threshold: 4h)" visible; timestamp displayed | Not run |
| **AT-05** | FR-09, NFR-04 | Offline with cached data (fixture at05) | Banner "Offline — showing cached data from 2026-10-14T06:00:00Z"; calculation works; data not presented as live | Not run |
| **AT-06** | FR-06, FR-07 | Weather missing (fixture at06) | "Weather data unavailable" displayed; no weather-dependent recommendation; comparison proceeds with available data | Not run |

---

## Execution & Recording

| Test | Run Date | Actual Result | Pass/Fail | Defects | Notes |
|------|----------|---------------|-----------|---------|-------|
| AT-01 | — | — | — | — | — |
| AT-02 | — | — | — | — | — |
| AT-03 | — | — | — | — | — |
| AT-04 | — | — | — | — | — |
| AT-05 | — | — | — | — | — |
| AT-06 | — | — | — | — | — |

**All tests marked "Not run" until implementation exists.** Round 1 requires test *definitions* with pass criteria; execution is for Build Round.

---

## Evaluation Plan (Beyond Acceptance Tests)

| Dimension | Method | Success Indicator |
|-----------|--------|-------------------|
| **Calculation correctness** | Manual verification of AT-01, AT-02 against independent calculator | 100% match |
| **Missing/stale data handling** | Execute AT-03, AT-04, AT-05; inspect UI warnings | All flags visible, no fabrication |
| **Explanation ↔ calculation consistency** | Compare AT-01/02 explanation text numbers to engine output | Zero discrepancies |
| **Understandability** | Proxy user (non-technical) walks through AT-02 scenario; identifies higher-net market & key assumption | >80% success rate |
| **Assumption visibility** | Check assumption panel shows: transport estimated, quote freshness, weather coverage | All present |
| **Connectivity loss behavior** | Simulate offline (DevTools); verify AT-05 behavior | Banner shown, calc works, no false freshness |

---

## Test Data Governance

- All fixtures in `tests/fixtures/` are **version-controlled**
- Fixtures labeled `simulated` — never presented as real data
- Freshness threshold configurable (default 4h) — not a universal agricultural standard
- Weather probability % shown as forecast probability, not system confidence