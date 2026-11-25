# Sprint 1 Initial Data Quality Audit
## Summary
This is first-pass exploration only. Formal cleaning will be completed in Sprint 2 & 3.

| Table | Finding | Severity | Business Impact | Next Action |
|---|---|---|---|---|
| inventory_master | current_stock vastly larger than max_stock / reorder_point | High | Cannot trust auto overstock flags | Validate unit definitions in Sprint 2 |
| stock_ledger | Wide date range (2019–2025) | Low | Long history, good for trend analysis | Filter date windows for modelling later |
| purchase_orders_lines | Total purchased quantity >> sales volume | High | Implies massive inventory build-up | Reconcile stock ledger in Sprint 2 |
| All tables | No critical PK nulls found | Low | Good | Monitor during joins |
| suppliers | Reliability score column does not match actual delivery rate | Medium | Supplier performance metric untrustworthy | Build custom reliability KPI in Sprint 7 |
| sales_orders | No cancelled order flag | Medium | Cannot exclude failed orders | Document assumption: all orders are fulfilled |
