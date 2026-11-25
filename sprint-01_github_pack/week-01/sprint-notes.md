# Sprint 1 Notes — Heavy Supplier & Warehouse Analytics

| | |
|---|---|
| **Sprint** | 1 of 12 (Week 1) |
| **Dates** | _[start]_ → _[end]_ |
| **Scrum Master** | _[name]_ |
| **Team** | DA1 _[name]_ · DA2 _[name]_ · DS _[name]_ |
| **Sprint goal** | Explore the full dataset, map table relationships, capture initial data quality issues, and agree the KPI frame and hypotheses for the 12 weeks |
| **Story points committed** | 12 |
| **Story points completed** | 12 |

## 1. Sprint goal (as agreed in planning)

> Load and understand all 12 tables, document the relational model and the first-pass quality
> picture well enough that Sprint 2 can start cleaning without re-discovering the data, and
> agree a shared KPI / hypothesis set that every later sprint reports against.

**No modelling or dashboards this sprint** — deliberately a discovery sprint.

## 2. Backlog and outcome

| ID | Task | Owner | Points | Status | Deliverable |
|---|---|---|---|---|---|
| T1.1 | Load all 12 CSVs, profile shape, dtypes, nulls, value ranges | DA1 | 2 | Done | `notebooks/data_exploration.ipynb`, `outputs/table_summary.csv` |
| T1.2 | Identify keys, verify joins, draw ERD, document grain | DA2 | 2 | Done | `docs/erd-map.md` |
| T1.3 | First-pass quality scan with quantified evidence and severity | DA1 + DA2 | 3 | Done | `docs/data-quality-findings.md`, `outputs/inventory_flags.csv` |
| T1.4 | Draft data dictionary from the supplied field guide + data | DS | 2 | Done | `docs/data-dictionary-draft.md` (12 tables, 134 fields) |
| T1.5 | Agree 12 KPIs with baselines and 6 testable hypotheses | All three | 2 | Done | `docs/kpi_hypotheses.md` |
| T1.6 | Run the Scrum ceremony, record decisions, risks and retro | Scrum Master | 1 | Done | this file + `outputs/purchase_vs_sales.csv`, `outputs/supplier_ontime.csv` |

Every task also produced something another sprint can reuse — the notebook is the seed of the
Sprint 2 cleaning pipeline, and the ERD fixes the join rules once instead of per-analysis.

## 3. What the data actually looks like

| Table | Rows | Columns | Grain |
|---|---|---|---|
| branches.csv | 6 | 13 | 1 per warehouse |
| customers.csv | 500 | 15 | 1 per customer |
| inventory_master.csv | 180 | 8 | 1 per product × branch |
| products.csv | 30 | 23 | 1 per SKU |
| suppliers.csv | 8 | 12 | 1 per supplier |
| sales_orders_header.csv | 20,000 | 11 | 1 per order |
| sales_orders_lines.csv | 130,402 | 9 | 1 per order line |
| purchase_orders_header.csv | 24,000 | 10 | 1 per PO |
| purchase_orders_lines.csv | 155,495 | 9 | 1 per PO line |
| invoices.csv | 18,033 | 10 | 1 per invoice |
| payments.csv | 19,257 | 5 | 1 per payment |
| stock_ledger.csv | 237,230 | 9 | 1 per stock movement |

Total: 605,141 rows across 12 tables, 134 fields, observation window
72.0 months (2019-01-01 → 2024-12-31 for orders; ledger to 2025-01-28).

## 4. Key findings

1. **The dataset is structurally clean.** No duplicate primary keys, no orphan order lines
   (130,402 sales lines and 155,495 purchase lines all resolve),
   no payments without an invoice, 0 negative ledger balances. Analysis can start
   without repair work.
2. **Stock parameters are not on the same scale as stock.** `current_stock` averages
   107,240 pieces against a `max_stock` average of 322 — a median ratio of
   349×. Consequently 180/180 SKU–branch combinations are flagged "overstock"
   and 0/180 "below safety stock". The ledger itself reconciles exactly to
   `current_stock` for all 180 combinations, so the defect is in the thresholds, not the movements.
3. **Purchasing is structurally decoupled from demand.** 22,387,123 units were received against
   1,234,939 units sold = **18.1×**; every SKU falls in a narrow
   17.4–19.2× band, which points to a fixed replenishment rule rather
   than isolated mistakes. Inventory value stands at 281,292,677,410.
4. **Supplier scoring has no signal.** Measured on-time delivery spans only
   84.2%–85.3%, while `reliability_score` (3–5) runs in the
   opposite direction to measured performance. Allocation and risk logic need a rebuilt score.
5. **Revenue concentration is real.** 9 A-class SKUs (30% of the catalogue) generate 78.5% of
   revenue; the single largest SKU is 21.2%. This gives Sprint 4 a defensible prioritisation order.

## 5. Quality issues carried into Sprint 2

| ID | Issue | Severity | Owner in Sprint 2 |
|---|---|---|---|
| DQ-01 | Stock thresholds vs stock scale (349× gap) | High | DA1 |
| DQ-02 | Purchase-to-sales ratio 18.1× | High | DA1 |
| DQ-03 | `invoice_id` not unique (196 duplicated IDs) | Medium | DA2 |
| DQ-04 | Payment amounts vs invoice status (155 overpaid, 17 contradictory) | Medium | DA2 |
| DQ-05 | Supplier score uncorrelated with on-time delivery | Medium | DS |
| DQ-06 | Duplicate reorder / safety thresholds in two tables | Low | DS |
| DQ-07 | `warehouse_capacity` stored as text | Low | DA2 |
| DQ-08 | ~10% order cancellations with no reason field | Low | DA1 |
| DQ-09 | Numeric-looking identifiers (pincode, IDs) | Low | DA2 |

Full evidence and proposed fixes: `docs/data-quality-findings.md`.

## 6. Decisions made in the sprint review

- **D1** — Revenue is defined as `total_order_value` (excl. GST) on `Delivered` orders. GST columns
  are never summed into revenue.
- **D2** — Stock health will be reported on **days-of-cover** until thresholds are recalibrated;
  the configured-threshold view stays as a secondary column so the defect remains visible.
- **D3** — Supplier performance is measured, not taken from `reliability_score`. The score becomes
  an input to be rebuilt in Sprint 7.
- **D4** — No data file is committed to the repository; `data/` is git-ignored and documented in
  `data/README.md` with the download link and row counts.

## 7. Blockers and risks

| Risk | Impact | Mitigation |
|---|---|---|
| Unit of `current_stock` unconfirmed (piece / box / pallet) | Every inventory KPI could be off by a constant factor | Publish both configured and measured views; ask the programme mentor before Sprint 4 |
| No business stakeholder to validate thresholds | Recalibration may be an assumption rather than a fact | Label recalibrated values as "analyst assumption" in the dictionary |
| Sales coverage possibly incomplete vs ledger | Purchase-to-sales ratio could be overstated | Reconcile ledger OUT movements against sales lines in Sprint 2/3 |
| Team time zone spread | Ceremony attendance | Fixed weekly check-in slot agreed at planning |

## 8. Retrospective

**What went well**
- All 12 tables loaded without parsing failures on the first pass.
- Profiling and ERD were done in parallel, so the quality scan could use verified joins immediately.
- The team converged on one revenue definition before any number was published.

**What was hard**
- Explaining the 349× stock-threshold gap; the first instinct (bad data) was only accepted after
  reconciling the ledger to `current_stock`.
- Keeping the sprint to discovery — several modelling ideas had to be parked into the hypothesis list.

**Improvement for Sprint 2 (one action)**
- Write the cleaning rules as code (one module with documented functions) rather than ad-hoc
  notebook cells, so the cleaned dataset is reproducible by any member.

## 9. Deliverables committed

```
week-01/
├── sprint-notes.md                       # this file
├── notebooks/data_exploration.ipynb      # executed, outputs included
├── outputs/
│   ├── table_summary.csv
│   ├── key_date_checks.csv
│   ├── inventory_flags.csv
│   ├── purchase_vs_sales.csv
│   └── supplier_ontime.csv
└── docs/
    ├── erd-map.md
    ├── data-dictionary-draft.md
    ├── data-quality-findings.md
    └── kpi_hypotheses.md
```

## 10. Next sprint (Sprint 2 preview)

Goal: turn the findings into a repeatable cleaning + integration layer.
Candidates: unit and threshold reconciliation (DQ-01), composite invoice key (DQ-03), payment
status rebuild (DQ-04), first integrated fact table at product × branch × month grain,
and the cleaned dataset written to `data/processed/`.
