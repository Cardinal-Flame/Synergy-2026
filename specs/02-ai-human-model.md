# AI & Human Responsibility Model

## Responsibility Separation

| Component | Responsibility | V1 Implementation | Must NOT Do |
|-----------|----------------|-------------------|-------------|
| **Deterministic Calculation Engine** | Net realization, total proceeds, ranking, timestamp checks, input validation, freshness evaluation | Pure functions (Python/JS), unit tested, no external deps | Invent prices, guess costs, make recommendations |
| **Data Retrieval Layer** | Fetch mandi quotes, weather forecasts, manage cache, preserve provenance metadata | Simulated dataset + optional prototype API adapter | Fabricate data, silently overwrite stale with guesses |
| **Data-Quality Checks** | Flag missing, stale (>threshold), conflicting, or incomplete data | Rule-based validators on every fetch/render | Auto-resolve conflicts, hide gaps |
| **AI Explanation Layer** | Generate plain-language comparison summary, translate (future), highlight trade-offs & assumptions | Template-based + LLM-assisted (English only V1) | Invent quotes, claim certainty on forecasts, make decisions |
| **Human Farmer (Decision-Maker)** | Verify practical details (actual quote obtainability, transport reality, crop condition), make final selling decision | UI presents options + warnings; no "recommend" button | Be replaced by automated decision |

## Decision Boundary (Critical)

```
┌────────────────────────────────────────────────────────────────────┐
│                        SYSTEM RESPONSIBILITY                        │
│  ✓ Calculate: "Market A: ₹19/kg net, Market B: ₹20/kg net"        │
│  ✓ Flag: "Market A quote 5hrs old (stale), Market B current"      │
│  ✓ Warn: "Transport cost for Market A is user-estimated"          │
│  ✓ Context: "IMD forecasts heavy rain in district (70%, 36hrs)"   │
│  ✓ Explain: "Despite lower headline price, Market B nets more"    │
└────────────────────────────────────────────────────────────────────┘
                           │
                    BOUNDARY LINE
                           │
┌────────────────────────────────────────────────────────────────────┐
│                       FARMER RESPONSIBILITY                         │
│  ✓ Verify: "Can I actually get ₹22/kg at Market B today?"         │
│  ✓ Assess: "Is my crop quality good enough for Market B?"         │
│  ✓ Decide: "I'll sell at Market B / wait / sell locally"          │
│  ✓ Bear: Consequence of decision (system provides info, not guarantee) │
└────────────────────────────────────────────────────────────────────┘
```

## AI Explanation Rules (V1)

### Must Do
- Use only validated data from deterministic engine
- Preserve all numerical values, units, and caveats exactly
- Highlight assumptions: "This comparison assumes your transport cost of ₹X is accurate"
- Note data gaps: "Market C quote missing — cannot compare"
- Note freshness: "Market A data is 5 hours old"
- Note uncertainty: "Weather forecast is district-level; your village may differ"
- Keep farmer as decision subject: "You may want to verify Market B's current price"

### Must NOT Do
- "I recommend selling at Market B" → **NO recommendations**
- "Rain will increase prices" → **NO causal claims without evidence**
- "You will earn ₹1,900" → **NO guaranteed outcomes** (use "estimated")
- "Market A is better because..." → **NO opaque ranking logic**
- Invent missing transport costs → **NO silent defaults**
- Translate numbers incorrectly → **Round-trip verification required**

## Connectivity Strategy (V1)

| Scenario | Behavior |
|----------|----------|
| **Online, fresh data** | Live fetch → calculate → explain → display |
| **Online, API fails** | Use cache (if < threshold) → banner "Using cached data from [time]" → calculate → explain |
| **Offline, cache valid** | Banner "Offline — cached data from [time]" → calculate → explain |
| **Offline, cache stale/empty** | Banner "Offline — no cached data" → allow manual entry → calculate on manual input → explain with "All data manually entered" warning |

**No SMS/IVR/WhatsApp in V1** — these are future accessibility modes pending U06 resolution.

## Trust & Transparency Mechanisms

| Mechanism | Purpose |
|-----------|---------|
| **Provenance on every number** | Source + timestamp + method (live/cached/manual/estimated) |
| **Assumption panel** | Collapsible list of all assumptions affecting the comparison |
| **No "best" badge** | Show ranked table; no green checkmark or "recommended" label |
| **Farmer verification prompt** | Before exit: "Have you verified the current quote at your chosen market?" |
| **Export/share** | Screenshot or text summary farmer can show to trader/aggregator for validation |

## Human-in-the-Loop for Authorised Advice

| Domain | System Role | Human Authority Required |
|--------|-------------|--------------------------|
| **Agronomic** (crop health, harvest timing, quality) | Flag weather context only | Licensed extension officer / KVK scientist |
| **Financial** (credit, insurance, large sales) | Show net-proceeds estimate only | Authorised financial advisor / bank officer |
| **Legal/Regulatory** (mandi rules, licensing) | Display market location only | Market committee / legal counsel |

**System never replaces authorised advice.** Disclaimer shown on weather and financial screens.

## AI Model Card (V1)

| Attribute | Value |
|-----------|-------|
| **Model** | Template-based + LLM (e.g., GPT-4o-mini) for explanation only |
| **Input** | Validated comparison JSON (deterministic output) |
| **Output** | English natural-language paragraph (≤200 words) |
| **Training data** | None (prompt-engineered, no fine-tuning) |
| **Evaluation** | Consistency check: explanation numbers == engine numbers |
| **Failure mode** | If LLM unavailable → fallback to template: "Market A nets ₹X/kg, Market B nets ₹Y/kg. Key assumption: transport costs as entered." |
| **Governance** | Prompt version-controlled; output logged for audit |

## Verification Checklist (AI/Human Boundary)

- [ ] Deterministic engine has zero LLM calls
- [ ] Explanation layer receives only validated engine output
- [ ] No code path writes "recommended", "best", "should sell"
- [ ] All numerical claims in explanation traceable to engine output
- [ ] Farmer decision recorded as explicit user action (not implicit)
- [ ] Weather text includes source, time, coverage, probability
- [ ] Offline mode never claims live data freshness