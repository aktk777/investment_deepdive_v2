# Phase 6: Macro & External Environment

## Objective
Place the individual stock analysis in macro context.
Even the best company can see its stock decline under macro headwinds (rising rates, recession, regulation).
Conversely, riding a thematic tailwind can drive significant capital inflows.

## Researcher: Information Gathering

### Interest Rate Environment
- BOJ policy rate and direction (for JP stocks)
- Fed funds rate and dot plot (for US stocks)
- Long-term rates (10Y government bond yields) — level and direction
- Yield curve shape (normal / flat / inverted)

### Economic Conditions
- GDP growth (actual and forecast)
- PMI (manufacturing / services)
- CPI (Consumer Price Index)
- Employment data

### Regulatory & Social Environment
- Regulatory changes affecting the company
- Political dynamics (elections, policy shifts)
- Social trends (demographics, work style changes, sustainability)

### Capital Flow Dynamics
- Growth vs. Value rotation
- Sector rotation (which sectors seeing inflows/outflows)
- Cross-border flows (foreign investor flows into JP stocks, EM → DM shifts)
- Cross-asset flows (equities vs. bonds vs. real estate vs. commodities)
- Thematic investment trends (AI, semiconductors, defense, GX, etc.)

### Recommended Search Queries
- `BOJ monetary policy latest 2026` / `Fed interest rate decision latest`
- `Japan GDP growth forecast` / `US GDP growth forecast`
- `sector rotation latest trends`
- `"{industry}" regulatory reform legislation`
- `AI investment trend 2026`
- `foreign investors Japan stocks flow`

### Priority Sources
- BOJ (Monetary Policy Meeting results), Fed (FOMC statements)
- Cabinet Office (Monthly Economic Report, GDP), BLS/BEA (US)
- Bloomberg, Reuters (macro analysis)

### Macro Trend Reference File
Read `assets/megatrend_reference.json`.
This file contains curated analysis of technology and social trends.
If the target stock relates to any covered trend, incorporate it into the analysis.

Key recurring themes in the file:
- AI compute infrastructure (GPU, memory, data centers) investment dynamics
- Generative AI / agent adoption impact on SaaS companies
- Policy, regulatory, and geopolitical risks (US-China, immigration, currency)
- Robotics / humanoid social deployment
- Major M&A / IPO event impacts

## Analyst: Evaluation

### Interest Rate Impact
- Rising rates → negative for growth stocks (higher DCF discount rate)
- Falling rates → positive for growth, also for RE/utilities
- Assess the target stock's interest rate sensitivity

### Sector & Thematic Tailwinds/Headwinds
- Is capital flowing into this stock's sector?
- Does the company ride themes like AI, semis, defense, GX?
- Also assess overheating risk (bubble formation in the theme)

### Geopolitical & Regulatory Risk
- Any direct regulatory changes affecting this business?
- Supply chain risk (China dependency, etc.)
- FX risk (for exporters)

### Catalyst Identification
Identify specific catalysts that could drive capital inflows:
- Earnings announcement dates
- New product/service launches
- M&A activity
- Stock splits, dividend increases
- Index inclusion
- Regulatory deregulation
- Theme momentum acceleration

## Internal Summary Format

```
【Phase 6: Macro & External Environment】
■ Rates: [Accommodative/Neutral/Tightening] — impact on stock: [Positive/Neutral/Negative]
■ Sector Flows: [Inflow/Outflow/Neutral]
■ Thematic Fit: [theme name] — [tailwind/headwind/irrelevant]
■ Key Risks: [geopolitical/regulatory/FX/economic etc]
■ Catalyst Candidates: [specific event + timing]
■ Macro Judgment: [Favorable/Neutral/Unfavorable environment]
```
