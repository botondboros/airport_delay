# European Airport Connection Delay Audit

**How much of your connection window could you actually lose?**

An interactive, data-driven breakdown of **37 European airports** across 4 delay touchpoints — for EU passport holders on Schengen transit connections. Built as a single standalone HTML file, no dependencies, no build step.

🔗 **[Live demo](https://botondboros.github.io/connection-delay-audit)**

---

## What it shows

Each airport is scored across 4 touchpoints that could consume your available connection window:

| Touchpoint | Type | What it measures |
|---|---|---|
| 🚶 Walking distance | Window cost | Typical gate-to-gate walk inside terminal (median gate pair, ~45% of worst case) |
| 🔍 Security re-screening | Window cost | X-ray + body scanner — **0 for EU passport on Schengen connections** |
| 🚌 Bus transfer | Window cost | Apron bus time, weighted by LCC remote stand exposure per airport |
| ✈️ Avg. inbound lateness | Flight delay | Expected mins the arriving flight is late — all causes, all carriers incl. LCC |

The **total** is the *potential connection delay in minutes* — a central estimate of how much of your connection window could already be consumed before you reach the departure gate.

---

## What's different: LCC remote stand adjustment

Standard airport factsheet data undercounts bus transfer time at airports dominated by low-cost carriers, where remote stands are the operational default rather than the exception. Bus transfer values in this dashboard reflect estimated LCC remote stand exposure:

| Airport | Est. LCC share | Key carriers | Bus value |
|---|---|---|---|
| BUD — Budapest | ~70% | Wizz Air hub, Ryanair | 8 min |
| CGN — Cologne | ~85% | Ryanair base | 7 min |
| BTS — Bratislava | ~90% | Wizz Air, Ryanair | 7 min |
| OTP — Bucharest | ~55% | Wizz Air hub, Ryanair | 8 min |
| PMI — Palma | ~60% | Ryanair, Wizz, charter | 6 min |
| BCN — Barcelona | ~50% via T2 | Vueling, Ryanair T2 | 5 min |
| PRG — Prague | ~45% | Ryanair T1, Wizz | 5 min |
| DUB — Dublin | ~60% | Ryanair hub T1 | 5 min |

OTP15 data (inbound lateness) includes all carriers including LCC. Ryanair and Wizz Air tend toward above-average punctuality, which partially offsets their higher bus transfer cost at LCC-dominated airports.

---

## Scope

- **37 airports** — 30 Schengen, 7 non-Schengen (LHR, LGW, MAN, STN, DUB, IST, BEG)
- **Passenger profile** — EU passport holders, transit (connecting) only
- **Estimate level** — central / mid-range, not worst case
- Non-Schengen airports are clearly marked; security re-screening values apply

---

## Methodology

### Inbound lateness
Derived from ACI-APN 2024 per-airport OTP15 (% of flights arriving within 15 min of schedule):

```
avg_delay ≈ 0.45 × (100 − OTP15%)
```

The `0.45` coefficient reflects a central operating scenario. The full statistical mean (~0.63) weights high-disruption days disproportionately. Calibrated at network average: 17.5 min / 72.4% OTP (CODA 2024).

### Walking distance
Median gate-pair (~45% of worst-case maximum pair), reflecting a typical connection rather than the unluckiest possible gate assignment. Walking speed: 1.2 m/s.

### Air traffic congestion (ATFM)
Shown as a sub-component in the detail view only — already embedded in inbound lateness and **not double-counted** in the total. Network average airport ATFM: 1.7 min/flight (CODA 2024).

---

## OTP15 values used (arrival punctuality, annual 2024)

**Verified — ACI-APN mandatory EU reporting:**
AMS 74.7% · ARN 78.6% · ATH 68.5% · BCN 74.9% · BRU 75.4% · CDG 74.9% · CPH 77.7% · DUB 71.5% · DUS 74.5% · FCO 71.8% · FRA 72.0% · LGW 63.1% · LHR 68.1% · LIS 63.4% · LYS 75.6% · MAD 78.1% · MUC 73.1% · MXP 70.3% · ORY 75.5% · OSL 80.2% · PMI 72.6% · PRG 70.9% · STN 64.1% · VIE 74.5% · ZRH 71.0% · BUD 67.1%

**Estimated (not in ACI dataset):**
HEL ~79% · WAW ~75% · HAM ~75% · NCE ~75% · OTP ~68% · BTS ~80% · CGN ~75% · MLA ~78% · IST ~72% · BEG ~70% · MAN ~69%

---

## Data sources

| Source | Used for | Recency | Access |
|---|---|---|---|
| [ACI-APN Europe Punctuality Report](https://www.aci-europe.org) | Per-airport OTP15 → inbound lateness | Annual 2024 + monthly 2025 | Free |
| [EUROCONTROL CODA Annual Report 2024](https://www.eurocontrol.int/publication/all-causes-delays-air-transport-europe-annual-2024) | Network avg delay, ATFM sub-values | 2024 | Free |
| [ansperformance.eu (EUROCONTROL PRU)](https://ansperformance.eu/data/) | Monthly airport ATFM delay time series | Monthly updated | Free |
| [CCC European Airport Index 2024](https://consumerchoicecenter.org/european-airport-index-2024/) | Security checkpoint wait time per airport | 2024 | Free |
| [OpenStreetMap Overpass API](https://overpass-api.de) | Gate positions → median walking distance | Ongoing | Free |
| [IATA Station Standard MCT](https://iata-codes.com/en/mct-checker) | Minimum connection time reference (I–I) | Current | Partly free |
| [ACI Europe Airport Factsheets](https://aci-europe.org) | Base remote stand / bus transfer data | Various | Free |

---

## Dashboard features

- **Sortable heatmap** — click any column header
- **Filter by region** — All / Schengen only / Non-Schengen only / WE / NE / SE / EE / Turkey
- **Absolute color scale** — 0 min = green, 30+ min = red, same meaning in every column
- **Detail panel** — click any airport row: waterfall breakdown, MCT buffer, ATFM sub-component, LCC share
- **Methodology tab** — all sources, quality indicators, LCC adjustment rationale documented inline

---

## How to deploy

### GitHub Pages (recommended)

1. Create repo `connection-delay-audit` (Public)
2. Upload `connection-delay-audit.html` and `README.md`
3. Settings → Pages → Source: `main` branch, root `/`
4. Live at: `https://yourusername.github.io/connection-delay-audit`

### Local

```bash
git clone https://github.com/yourusername/connection-delay-audit.git
open connection-delay-audit.html
```

No server, no npm, no build tools needed.

---

## File structure

```
connection-delay-audit/
├── connection-delay-audit.html   # Complete self-contained dashboard
└── README.md
```

---

## Data quality indicators

| Label | Meaning |
|---|---|
| ✅ verified | Directly measurable from mandatory EU reporting |
| 🟡 estimated | Public methodology, no airport-specific measurement |
| 🔵 modelled | Derived from verified data via calibrated formula |
| ⚪ sub-component | Included in another column, not added to total |

---

## Author

Built by [Botond Boros](https://botondboros.github.io) — Head of BI & Digital Solutions at Visit Hungary.

Part of an ongoing series of open data portfolio projects using the **AI Indiana Jones** methodology: open question + structured data + AI-assisted pattern recognition + public narrative.

---

## License

Code: MIT  
Underlying data: subject to the terms of each source (all publicly accessible, non-commercial use).
