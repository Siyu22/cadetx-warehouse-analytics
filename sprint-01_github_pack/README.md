# CadetX — Heavy Supplier, Inventory & Warehouse Analytics

Applied analytics programme (12 weeks, 12 agile sprints, team of 3).
Analysing heavy-goods supplier, product, inventory and warehouse transaction data to build
a data-driven warehouse intelligence capability.

| Item | Detail |
|---|---|
| Duration | 3 months · 12 weekly sprints |
| Cadence | Weekly Scrum (planning → build → check-in → submit → review) |
| Delivery | GitHub submission every week + weekly team meeting |
| Dataset | Heavy Suppliers / Warehouse / Inventory / Customer (12 CSV tables) |

## Business problem
Warehouses and supply-chain teams operate reactively. Raw operational data exists, but it is
not structured into decisions, which produces stock imbalance, overstocking, stockouts,
poor space use, unreliable suppliers and inaccurate demand forecasts.

This repository builds the analytical layer that answers five questions:

1. How do products move, and what do demand patterns look like?
2. How healthy is inventory, and how well is it utilised?
3. How efficient are warehouse operations?
4. Where are the stock risks and operational waste?
5. What should procurement and planning do differently?

## Repository structure

```
.
├── README.md
├── requirements.txt
├── .gitignore
├── data/                    # raw CSVs (not committed - see data/README.md)
├── week-01/                 # Sprint 1: discovery & foundation
│   ├── sprint-notes.md
│   ├── notebooks/data_exploration.ipynb
│   ├── outputs/             # generated CSVs from the exploration notebook
│   └── docs/
│       ├── erd-map.md
│       ├── data-dictionary-draft.md
│       ├── data-quality-findings.md
│       └── kpi_hypotheses.md
├── week-02/                 # Sprint 2: cleaning & integration
├── week-03/                 # Sprint 3: feature engineering
├── dashboards/
├── models/
└── docs/
```

## Sprint roadmap

| Sprint | Weeks | Focus |
|---|---|---|
| 1 | 1 | Dataset discovery, ER mapping, quality scan, KPI & hypothesis alignment |
| 2–3 | 2–3 | Cleaning, integration, feature engineering, final data dictionary |
| 4–6 | 4–6 | Product, inventory & warehouse analytics |
| 7–9 | 7–9 | Supplier risk, RFM segmentation, demand forecasting |
| 10–12 | 10–12 | Dashboards, KPI framework, anomaly detection, strategy, final presentation |

## Headline numbers from Sprint 1

- 12 tables, 605,141 rows analysed
- 22.96 bn revenue (2019–2024, delivered orders, excl. GST)
- 281.3 bn inventory value across 180 SKU–warehouse combinations
- Purchases run **18.1×** ahead of delivered sales — the root cause of stock build-up
