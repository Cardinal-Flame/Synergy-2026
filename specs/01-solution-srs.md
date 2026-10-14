# Solution Concept, SRS & Implementation Plan

## 1. Solution Concept

### 1.1 Workflow (4 Steps)

```
┌─────────────────────────────────────────────────────────────────┐
│ STEP 1: Enter Selling Context                                   │
│ • Crop & variety                                                │
│ • Quantity available to sell (kg)                               │
│ • Farmer location (village/cluster)                             │
│ • Harvest condition & intended selling window                   │
└──────────────────────────┬──────────────────────────────────────┘
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│ STEP 2: Retrieve & Validate Available Data                      │
│ • Mandi quotes: price/kg, source, timestamp                     │
│ • Transport & loading cost estimates (user-entered or cached)   │
│ • Weather forecast: condition, probability, source, update time │
│ • Data-quality checks: missing, stale, conflicting              │
└──────────────────────────┬──────────────────────────────────────┘
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│ STEP 3: Calculate & Compare (Deterministic Engine)              │
│ • Gross proceeds = Quantity × Quoted Price/kg                   │
│ • Selling costs/kg = (Transport + Loading + Unloading) / Qty    │
│ • Net realization/kg = Quoted Price/kg − Selling Costs/kg       │
│ • Total net proceeds = Quantity × Net Realization/kg            │
│ • Rank markets by total net proceeds (where data sufficient)    │
└──────────────────────────┬──────────────────────────────────────┘
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│ STEP 4: Explain & Decide                                        │
│ • Plain-language comparison table                               │
│ • Key assumptions & uncertainties highlighted                   │
│ • Weather context with source/timestamp                         │
│ • Data freshness & connectivity status                          │
│ • Farmer verifies practical details → makes final decision      │
└─────────────────────────────────────────────────────────────────┘
```

### 1.2 Data Model

**Input Fields (per market):**
| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `market_id` | string | Yes | Unique identifier |
| `market_name` | string | Yes | Display name |
| `market_location` | string | Yes | Village/district |
| `distance_km` | number | Yes | From farmer location |
| `crop` | string | Yes | Must match farmer's crop |
| `variety` | string | No | If applicable |
| `quoted_price_per_kg` | number | Yes | From source |
| `quote_source` | string | Yes | e.g., "eNAM", "trader", "simulated" |
| `quote_timestamp` | ISO8601 | Yes | Freshness assessment |
| `transport_cost_total` | number | Yes | User estimate or cached |
| `loading_cost_total` | number | No | Default 0 if unknown |
| `unloading_cost_total` | number | No | Default 0 if unknown |
| `weather_forecast` | object | No | See below |

**Weather Forecast Object:**
```json
{
  "condition": "heavy_rain",
  "probability_percent": 70,
  "valid_from": "2026-10-14T18:00:00Z",
  "valid_until": "2026-10-15T18:00:00Z",
  "source": "IMD",
  "source_timestamp": "2026-10-14T06:00:00Z",
  "geographic_coverage": "district"
}
```

**Output Fields (per market):**
| Field | Type | Notes |
|-------|------|-------|
| `market_id` | string | |
| `quoted_price_per_kg` | number | |
| `selling_costs_per_kg` | number | (transport + loading + unloading) / qty |
| `net_realization_per_kg` | number | quoted − selling_costs |
| `total_net_proceeds` | number | qty × net_realization |
| `data_freshness` | enum | `current` \| `stale` \| `missing` |
| `warnings` | string[] | e.g., "quote > 4hrs old", "transport estimated" |

### 1.3 Core Calculation (Deterministic)

```python
def calculate_net_proceeds(
    quantity_kg: float,
    quoted_price_per_kg: float,
    transport_cost_total: float,
    loading_cost_total: float = 0,
    unloading_cost_total: float = 0
) -> dict:
    """
    Pure function — no external dependencies, fully reproducible.
    """
    selling_costs_per_kg = (transport_cost_total + loading_cost_total + unloading_cost_total) / quantity_kg
    net_realization_per_kg = quoted_price_per_kg - selling_costs_per_kg
    total_net_proceeds = quantity_kg * net_realization_per_kg
    
    return {
        "selling_costs_per_kg": round(selling_costs_per_kg, 2),
        "net_realization_per_kg": round(net_realization_per_kg, 2),
        "total_net_proceeds": round(total_net_proceeds, 2)
    }
```

**Illustrative Test (AT-01):**
- Quote: ₹24/kg, Selling costs: ₹5/kg, Quantity: 100 kg
- Net realization: ₹19/kg, Total net proceeds: ₹1,900
- *These are example inputs, not market recommendations.*

---

## 2. Software Requirements Specification (SRS)

### 2.1 Functional Requirements

| ID | Requirement | Priority | Verification |
|----|-------------|----------|--------------|
| **FR-01** | Accept crop, quantity, location, and selling-window inputs | Must | Valid/invalid input tests |
| **FR-02** | Display mandi quotes with source and timestamp | Must | Inspect output against source records |
| **FR-03** | Calculate estimated net realization after included selling costs | Must | Independently recalculate sample outputs (AT-01) |
| **FR-04** | Compare estimated total net proceeds where sufficient data exists | Must | AT-01, AT-02 pass |
| **FR-05** | Identify missing, stale, or conflicting data | Must | AT-03, AT-04, AT-05 pass |
| **FR-06** | Explain the comparison and its key assumptions | Must | AT-06 (explanation consistency) |
| **FR-07** | Show relevant weather information with source and update time | Should | Manual inspection |
| **FR-08** | Support English UI (Hindi = future) | Should | Language test |
| **FR-09** | Display cached information with clear freshness warning | Should | AT-05 pass |
| **FR-10** | Selling-time comparisons using crop-condition/storage data | Future | Not in V1 |

### 2.2 Non-Functional Requirements

| ID | Requirement | Target / Notes |
|----|-------------|----------------|
| **NFR-01** | **Correctness**: Deterministic engine returns reproducible results for valid inputs | 100% pass on AT-01, AT-02 |
| **NFR-02** | **Transparency**: Each estimate identifies source, timestamp, included costs, assumptions | All FR-02, FR-05 outputs auditable |
| **NFR-03** | **Usability**: Farmer understands comparison without technical terminology | Evaluation plan: proxy user testing |
| **NFR-04** | **Resilience**: Handles failed requests, connectivity loss, unavailable quotes without fabrication | AT-03, AT-05 pass |
| **NFR-05** | **Privacy**: Minimise personal data collection; no unnecessary tracking | No login, no PII stored |
| **NFR-06** | **Accessibility**: Mobile-first, low-end device compatible, readable font sizes | Test on 320px width, 2G simulation |
| **NFR-07** | **Maintainability**: Retrieval, calculation, explanation layers separable | Module boundaries in architecture |
| **NFR-08** | **Language reliability**: Translations preserve numerical values, units, caveats | V1: English only; future: verified Hindi |

### 2.3 Data Requirements

| Requirement | Detail |
|-------------|--------|
| **Primary data (V1)** | Labelled simulated dataset (`tests/fixtures/`) — reproducible, version-controlled |
| **Prototype integration** | Live mandi API *if* access verified (coverage, freshness, rate limits documented) |
| **Weather integration** | Forecast API with timestamp + geographic coverage label |
| **Fallback** | Cached quotes (max age configurable, default 4 hrs) + manual entry |
| **Units** | Price: ₹/kg, Quantity: kg, Distance: km, Costs: ₹ total |

### 2.4 Error Handling (Explicit Cases)

| Error Condition | System Behavior |
|-----------------|-----------------|
| Market quote missing | Mark `data_freshness: "missing"`; exclude from ranking; show in table with "—" |
| Conflicting prices from multiple sources | Show all with sources; flag conflict in warnings; do not auto-resolve |
| Transport estimate unavailable | Prompt user entry; if skipped, mark market as "costs unknown" — no ranking |
| Quantity ≤ 0 | Reject input with clear error message |
| Unit mismatch (e.g., price in ₹/quintal) | Detect or warn; convert if standard, else flag |
| Weather service fails | Show "weather unavailable" with timestamp of last attempt; no forecast display |
| Cached quote exceeds freshness threshold | Show with `data_freshness: "stale"` + warning; do not use for ranking if current exists |
| AI explanation service unavailable | Show deterministic comparison table; explanation = "Service unavailable — review numbers directly" |
| Network connectivity lost | Use cached data; banner: "Offline — showing cached data from [timestamp]"; calculations still work |

### 2.5 Version 1 Boundary

| Included in V1 | Excluded from V1 |
|----------------|------------------|
| Net-proceeds comparison for 1 crop, ≤5 mandis | Multi-crop, nationwide mandi coverage |
| Deterministic calculation engine | ML-based price prediction |
| Simulated/test dataset with provenance | Live API dependency (prototype only) |
| Source/timestamp/freshness on every quote | Real-time streaming quotes |
| Stale/missing/conflict data flags | Auto-resolution of conflicts |
| Weather context (source, time, coverage) | Harvest-timing recommendation |
| Offline cached data with warnings | SMS/IVR/WhatsApp/voice interfaces |
| English UI | Hindi/local language UI |
| Farmer decision authority explicit | Autonomous decision-making |
| Transparent assumptions & limitations | Guaranteed outcomes (price, income, loss reduction) |

---

## 3. Implementation Plan (Phased for Build Round)

| Stage | Duration | Deliverable | Dependencies |
|-------|----------|-------------|--------------|
| **1. Finalize Scope** | 1 day | Frozen problem statement, evidence ledger, V1 boundary | Case analysis complete |
| **2. Data & Calculation Core** | 2 days | Deterministic engine (tested), data schemas, simulated fixture dataset | FR-01, FR-03, FR-04 |
| **3. Retrieval & Freshness** | 2 days | Quote fetcher (simulated + prototype API), cache layer, freshness checker | FR-02, FR-05, FR-09 |
| **4. Comparison UI** | 3 days | Mobile React app: input form, comparison table, warnings, weather panel | FR-01, FR-02, FR-04, FR-07, FR-08 |
| **5. Explanation Layer** | 2 days | AI-assisted natural-language summary (template-based V1), consistency checks | FR-06, NFR-02, NFR-08 |
| **6. Resilience & Offline** | 2 days | Service worker cache, offline banner, graceful degradation | FR-09, NFR-04, AT-05 |
| **7. Acceptance Testing** | 2 days | Execute AT-01 to AT-06, record defects, fix critical | All FRs, NFRs |
| **8. Documentation & Trail** | 1 day | AI interaction log, decision index, existing-component disclosure, SUBMISSION.md | All prior stages |

**Total estimated**: ~15 working days for Build Round prototype.

**Milestones for Round 1 Submission:**
- M1: Problem statement frozen (Day 1)
- M2: SRS + architecture signed off (Day 3)
- M3: Deterministic engine + test fixtures passing (Day 5)
- M4: UI + explanation integrated, ATs passing (Day 12)
- M5: Documentation package complete (Day 15)

---

## 4. Data Source Strategy (V1)

| Tier | Source | Role | Verification |
|------|--------|------|--------------|
| **1. Primary (testing)** | `tests/fixtures/market_data.json` | Reproducible AT execution | Version-controlled, manually verified |
| **2. Prototype** | eNAM / state mandi API (if accessible) | Live quote demonstration | Access verified, coverage mapped, rate limits known |
| **3. Weather** | IMD / OpenWeatherMap API | Contextual forecast | Timestamp + coverage label preserved |
| **4. Fallback** | LocalStorage cache (max 4hr default) | Offline/intermittent operation | Freshness warning always shown |

**Principle**: A reproducible test dataset is more valuable for Round 1 than an unreliable live API demo. Live integration is a "should have" prototype, not a "must have" for V1 correctness.