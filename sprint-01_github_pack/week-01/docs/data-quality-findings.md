# Data Quality Findings — Sprint 1

First-pass audit only. Cleaning rules, fixes and the cleaned dataset are Sprint 2–3 deliverables.
Every number below is reproducible from `week-01/notebooks/data_exploration.ipynb`.

**Summary:** 10 findings — 2 high, 3 medium, 4 low, 1 informational note.
The dataset is structurally sound (keys resolve, no orphan lines, no negative stock), but its
**stock parameters and procurement behaviour are internally inconsistent**, so inventory
conclusions cannot be trusted until those two issues are resolved.

## Findings

| ID | Severity | Table / field | Finding | Evidence | Business impact | Action |
|---|---|---|---|---|---|---|
| DQ-01 | **High** | `inventory_master.current_stock` vs `max_stock`, `opening_stock` | Stock quantity and stock thresholds are on different scales, so standard overstock logic returns garbage | `current_stock` mean 107,240 vs `max_stock` mean 322 → median ratio 349×; 180/180 combinations exceed `max_stock`, 0/180 fall below `safety_stock` | Every reorder-point / safety-stock / overstock rule is unusable; overstock counts are meaningless | Recalibrate thresholds from 12-month demand; confirm the unit of `current_stock` (piece / box / pallet); until then judge stock by days-of-cover only |
| DQ-02 | **High** | `purchase_orders_lines.quantity` vs `sales_orders_lines.quantity` | Purchases run systematically ahead of delivered sales | 22,387,123 units received vs 1,234,939 units sold = **18.1×**; every one of the 30 SKUs falls between 17.4× and 19.2× | Stock builds up permanently; 281.3 bn of value is tied up, and the pattern is a rule/process failure rather than a few mis-ordered SKUs | Audit the replenishment rule (fixed lot size vs demand-driven?); test "monthly demand × cover days"; freeze routine purchasing on SKUs above 18× |
| DQ-03 | Medium | `invoices.invoice_id` | Invoice ID is not unique | 18,033 rows / 17,836 unique IDs; 196 IDs appear twice on different sales orders, customers and amounts | Joining payments on `invoice_id` alone double-counts receivable balances; DSO and ageing become wrong | Use `invoice_id + so_id` as the composite key; raise a data-owner ticket to reissue numbers |
| DQ-04 | Medium | `payments.payment_amount`, `invoices.payment_status` | Payment amounts contradict invoice status | 155 invoices are paid above their invoice total; 17 invoices marked `Unpaid` carry payments | Collection rate, overdue rate and DSO cannot be taken at face value | Rebuild status from payment facts; apply a ±1 tolerance for "fully paid"; route overpayments to manual write-off review |
| DQ-05 | Medium | `suppliers.reliability_score` | Score does not reflect actual delivery performance | Measured on-time rate spans 84.2%–85.3% (1.1 pp spread) while scores range 3–5 with no monotonic link (lowest-scored supplier has the best on-time rate) | Supplier tiering, allocation and risk alerts have no signal | Rebuild the score from measured on-time %, average delay, cancellation rate and price variance; re-rate quarterly |
| DQ-06 | Low | `products` vs `inventory_master` stock parameters | Two conflicting sets of reorder / safety / max thresholds | Product level e.g. `reorder_level` 15, `max_stock_level` 40; inventory level 59 / 340 for the same SKU | Ambiguity about which rule replenishment actually uses | Declare one source of truth (policy at product level, execution at inventory level) and document precedence |
| DQ-07 | Low | `branches.warehouse_capacity` | Capacity stored as text with a unit suffix | All 6 rows look like "45230 sqft" | Space utilisation and revenue-per-sqft cannot be computed without parsing | Split into `capacity_value` + `capacity_unit` |
| DQ-08 | Low | `purchase_orders_header` / `sales_orders_header` status | ~10% cancellations with no reason captured | 2,370/24,000 POs cancelled (9.9%), 1,967/20,000 SOs cancelled (9.8%); no `cancel_reason` field | The 10% rate is itself a process-loss signal but cannot be attributed | Add a reason field at source; exclude cancellations from volume KPIs meanwhile |
| DQ-09 | Low | Identifier fields (`pincode`, `*_id`) | Numeric-looking identifiers can be coerced to numbers | 6-digit pincodes; `C0001` / `P001` style IDs | Leading zeros lost, IDs turned into floats in exports | Read and store as text; format `@` in Excel exports |
| DQ-10 | Info | All tables | Core integrity checks pass | See table below | No repair needed before analysis | Re-run these checks every week |

## Integrity checks that passed

| Check | Result |
|---|---|
| Duplicate primary keys (branches, customers, products, suppliers, SO, PO, payments, ledger) | 0 |
| Sales order ↔ line orphans | 0 of 130,402 lines |
| Purchase order ↔ line orphans | 0 of 155,495 lines |
| Payments with unknown invoice | 0 of 19,257 payments |
| Negative ledger running balance | 0 of 237,230 movements |
| Inventory coverage | 180 = 30 SKUs × 6 branches, no missing combination |
| `current_stock` vs ledger closing balance | identical for all 180 combinations (ratio 1.000) |

## Data range used for time-based analysis

`stock_ledger` runs 2019-01-01 → 2025-01-28; orders and invoices cover 2019-01-01 → 2024-12-31.
Sprint 1 uses 72.0 months as the observation window.
