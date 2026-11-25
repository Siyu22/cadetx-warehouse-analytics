# KPIs & Project Hypotheses — Sprint 1

The list below is the team's agreed measurement frame for the 12 weeks. Every baseline is
computed from the raw CSVs in Sprint 1, so later sprints can show movement against a fixed start point.

## Core KPI set

| # | KPI | Definition / formula | Source tables | Sprint 1 baseline | Direction of travel |
|---|---|---|---|---|---|
| 1 | Revenue (period) | Σ `total_order_value` where `order_status = Delivered` | sales_orders_header | 22,958,987,430 (2019–2024); 3,677,995,560 in 2024 | hold / grow |
| 2 | Gross margin % | (Revenue − Σ qty × `unit_cost`) / Revenue | sales lines + products | 21.6% | up |
| 3 | Inventory value | Σ `current_stock` × `unit_cost` | inventory_master + products | 281,292,677,410 | down sharply |
| 4 | Inventory turnover | annual COGS / inventory value | sales lines + inventory | ≈0.011 turns / yr | up to 4–6 |
| 5 | Days of inventory on hand | stock / monthly demand × 30.44 | inventory + sales | ≈34,250 days (≈1,125 months) | down to 30–90 |
| 6 | Overstock rate | share of SKU–branch combos above `max_stock` | inventory_master | 100% (threshold itself unreliable — DQ-01) | recalibrate first |
| 7 | Stockout / below-safety rate | share of combos below `safety_stock` | inventory_master | 0% | keep near 0 but on valid thresholds |
| 8 | Purchase-to-sales ratio | units received / units delivered | PO lines + SO lines | 18.1× | down toward 1.0–1.2 |
| 9 | Supplier on-time delivery % | received within promised `lead_time_days` | PO header + suppliers | 84.9% (range 84.2–85.3%) | up, with differentiation |
| 10 | Warehouse revenue per sqft (monthly) | Σ `avg_monthly_revenue` / Σ capacity | branches | ₹104 | up |
| 11 | Slow-moving / dead stock % | value share with no sales in 90 / 180 days | stock ledger + sales | to compute in Sprint 5 | down |
| 12 | Customer repeat rate & AOV | orders per customer; revenue per order | SO header | AOV ₹1,109,166 (2024); 500 customers all active | grow value, not just count |

Two KPI families are deliberately deferred: forecasting accuracy (needs a model, Sprint 8) and
space utilisation in m² (needs unit parsing, Sprint 4).

## Hypotheses to test

| ID | Hypothesis | Sprint 1 status | Evidence so far | Test in |
|---|---|---|---|---|
| H1 | A small subset of SKUs drives most revenue (Pareto) | **Supported** | 9 A-class SKUs (30% of catalogue) carry 78.5% of revenue; the top SKU alone is 21.2% | Sprint 4 |
| H2 | Warehouses differ materially in space and throughput efficiency | **Not supported yet** | Operating margin sits in a 37.5–39.1% band across all 6 branches; capacity and revenue are almost perfectly proportional | Sprint 5 |
| H3 | Supplier reliability varies enough to drive allocation decisions | **Rejected on current data** | On-time rate spread is 1.1 pp and scores are uncorrelated with it (DQ-05) — reliability must be rebuilt before it can differentiate | Sprint 7 |
| H4 | Much of the catalogue is overstocked relative to demand | **Supported (direction), magnitude unresolved** | Purchases run 18.1× ahead of sales on every SKU; cover is ≈34,250 days | Sprint 4–6 |
| H5 | Customer behaviour segments cleanly with RFM | **Supported** | R/F/M quintiles produce 7 distinct segments; 62 customers are "At Risk" with above-median frequency | Sprint 7 |
| H6 | Stock levels can be forecast from 6 years of movement history | **Open** | 237k movements with monthly seasonality; but current baseline is distorted, so forecast demand rather than stock first | Sprint 8 |

## Questions the business must answer before Sprint 4

1. **Unit of measure.** Is `current_stock` in pieces, boxes or pallets? The ~349× gap against
   `max_stock` is consistent with a unit mismatch, not with real overstock.
2. **Procurement intent.** Is the 18× purchase-to-sales ratio deliberate (contract buying,
   buffer building) or an uncontrolled fixed-lot rule?
3. **Sales coverage.** Does `sales_orders_lines` capture every outbound movement recorded in
   `stock_ledger`, or only invoiced sales?
4. **Threshold ownership.** Which system owns `reorder_level` / `safety_stock` — product master
   or warehouse inventory?

Until 1–4 are answered, Sprint 4 analyses will publish **two views** (configured thresholds vs
measured demand) and label which one each number uses.

## Definition of done for this file

- [x] 12 KPIs defined with formula, source and baseline
- [x] 6 hypotheses stated with a testable sprint and current status
- [x] Open business questions logged
- [ ] Reviewed and signed off by all three team members at the Sprint 1 review
