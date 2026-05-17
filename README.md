# Procurement Threshold Analysis
### Detecting systematic concentration patterns in Portuguese public contracts

This repository contains the full methodology and outputs of a statistical
analysis of 1,165,883 public procurement contracts from Portugal, spanning
2018 to 2025. The central question is whether contracts distribute randomly
around the legal thresholds defined by the Public Contracts Code (CCP) or
whether there is systematic concentration just below them.

The year 2017 is retained exclusively for temporal comparison as a pre-reform
baseline. The €20,000 threshold only came into full force in January 2018
with Decree-Law 111-B/2017.

---

## Key findings

- There are **2.7x more contracts** in the band immediately below the €20,000
  threshold than the window average, with an abrupt drop immediately after
  the limit. The same pattern appears independently at the €75,000 threshold.

- In 2017, before the reform, the ratio was **1.1x** — close to a random
  distribution. From 2018 onwards it jumps to 3.1x and reaches 4.7x by 2025,
  never returning to baseline. The threshold did not reveal a pre-existing
  pattern. It created one.

- **69 entity-supplier pairs** were flagged as showing extreme concentration
  in the risk band, with near-threshold rates between 87% and 100% against
  a market baseline of 5.8%. A further 512 pairs are in observation.

- The pattern is present in every district of mainland Portugal and across
  every type of public entity, confirming it is a structural response to the
  law's design rather than the behaviour of any specific sector or region.

**This analysis detects statistical patterns, not intent. No finding
constitutes evidence of wrongdoing. Every flagged case requires human
review before any conclusion can be drawn.**

---

## Legislative context

Portuguese public procurement law (CCP) defines spending thresholds that
determine which procedure an entity must follow:

| Threshold | Procedure | Competition required |
|-----------|-----------|----------------------|
| Below €20,000 | Ajuste Direto | None — single supplier |
| €20,000 to €75,000 | Consulta Prévia | At least 3 suppliers |
| Above €75,000 | Open tender | Full public competition |

Decree-Law 111-B/2017, in force from January 2018, consolidated the €20,000
limit as the clear boundary for direct award contracts. Decree-Law 112/2025,
in force from October 2025, raised this limit to €30,000. Data from 2026
onwards will be the most direct test of this study's hypothesis: if the
concentration pattern shifts to the new limit, it confirms that the threshold
itself drives the behaviour.

---

## Repository structure

    procurement-threshold-analysis/
    ├── notebooks/
    │   └── procurement_threshold_analysis.ipynb
    └── outputs/
        ├── medium/
        │   ├── medium_01_threshold_kde.png
        │   └── medium_02_temporal_evolution.png
        ├── market_thresholds.csv
        ├── market_yearly.csv
        ├── market_geographic.csv
        ├── flagged_pairs.csv
        ├── observation_pairs.csv
        ├── entity_summary.csv
        └── entity_type_summary.csv

---

## Methodology

### Data source

Source data is publicly available at BASE.gov.pt via dados.gov.pt (Domínio Público).
Download the annual contract files for 2017 to 2025 and place them in a data/ folder
at the root of the repository before running the notebook.

    data/
    ├── contratos2017.xlsx
    ├── contratos2018.xlsx
    ├── ...
    └── contratos2025.xlsx

### Scope filters

The analysis retains only contracts for goods and services
(Aquisição de bens móveis, Aquisição de serviços, Locação de bens móveis),
excluding public works (different threshold applies) and framework call-offs
(pre-competed, do not reflect threshold decisions).

### Threshold concentration (Sections 3 and 4)

For each legal threshold, we define a symmetric window of 25% above and
below the limit. Within this window, we measure whether the 5% band
immediately below the threshold contains significantly more contracts than
the average band of equivalent width. A chi-square goodness-of-fit test
confirms the non-uniformity of the distribution.

### Pair scan (Section 5)

We aggregate all contracts by entity-supplier pair and compute a
near-threshold rate using a 10% risk band (wider than the market analysis
to ensure statistically robust samples at the pair level). Pairs are
compared against the market baseline using a z-score. Flagged pairs satisfy
four simultaneous criteria:

1. At least 8 contracts in the pair
2. Near-threshold rate above 50%
3. Z-score above 2.0 relative to the market baseline
4. At least 3 contracts in the risk band

Pairs that show above-average concentration without meeting all four criteria
are classified as in observation.

---

## How to reproduce

Requirements:

    pip install pandas numpy scipy matplotlib seaborn pyarrow python-calamine tqdm

Steps:

1. Download the annual contract files from BASE.gov.pt and place them in data/
2. Open notebooks/procurement_threshold_analysis.ipynb
3. Run Section 2 once with CONVERSION_DONE = False to convert xlsx to parquet
4. Set CONVERSION_DONE = True and run all sections

The notebook is self-contained. All outputs are written to outputs/.

---

## Data

- Source: BASE.gov.pt via dados.gov.pt
- License: Domínio Público (Public Domain)
- Period: 2017 to 2025
- Records: 1,606,131 loaded · 1,165,883 after scope filters (2018-2025)

---

## Author

Thiago Chicó
https://github.com/ThiagoCh1

---

## Disclaimer

This analysis identifies statistical anomalies in publicly available
procurement records. It does not establish intent, legal violation, or
wrongdoing of any kind. All flagged cases require human review before
any conclusion can be drawn. The methodology and all outputs are fully
reproducible from public data.
