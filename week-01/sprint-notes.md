# Sprint 1 Notes | Heavy Supplier & Warehouse Analytics
Date: Sep 30 – Oct 6, 2026
Scrum Master: [Name]

## Sprint Goal
Explore the full supply chain dataset, map table relationships, flag initial data quality issues, define draft business KPIs & analysis questions to guide the full 12-week project.

## Sprint Backlog & Progress
| Task ID| Notes |
|---|---|
| T1.1 | Load all 12 CSV files, generate summary stats, shape, dtypes, describe |
| T1.2 | Build ERD and table join mapping |
| T1.3 | First-pass data quality scan: nulls, duplicates, outliers, invalid dates |
| T1.4 | Draft data dictionary from the official field documentation |
| T1.5 | Team alignment on core KPIs and research hypotheses |
| T1.6 | Final sprint notes, retrospective & prepare GitHub submission |

## Key Findings
1. Stock ledger date range: 2019-01-01 to 2025-01-28, ~237k transaction records
2. `current_stock` values in inventory_master are extremely large compared to max_stock / reorder_point thresholds
3. All 12 CSV tables loaded successfully, primary keys identified for most tables
4. Mismatched scale between stock quantity and configured stocking limits requires business validation

## Data Quality Issues Identified
| Issue | Count | Severity | Impact |
|---|---|---|---|
| Extreme outlier stock values in inventory_master | 180 | High | Causes automatic overstock misclassification |
| No clear last-updated timestamp for inventory master | N/A | Medium | Cannot confirm recency of static inventory snapshot |
| Check for duplicate order lines pending validation | TBC | Medium | Risk of inflated sales / purchase volume |
| Missing supplier contact metadata | TBC | Low | Not blocking core analysis |

## Agreed Draft KPIs & Research Questions
KPIs:
1. Inventory turnover rate
2. Days of inventory on hand
3. Supplier on-time delivery rate
4. Warehouse throughput
5. Slow-moving / dead stock percentage
6. Average customer order value
7. Stockout frequency
8. Space utilisation per warehouse

Research Questions:
1. Are current stock levels realistic compared to max_stock and reorder points?
2. Which product categories drive the majority of inventory holding value?
3. Do suppliers differ significantly in delivery reliability and risk?
4. How consistent is stock movement across different warehouse branches?
5. Can we separate fast-moving, slow-moving and dead stock from transaction history?

## Blockers & Risks
- Risk: Unit ambiguity (units could be each, box, pallet; need to confirm in later sprints)
- Risk: The large stock values may be unit misalignment, not genuine overstock
- Blocker: No business stakeholder available to validate thresholds

## Retrospective
What went well:
- All datasets loaded without parsing errors
- Team agreed on scope and priority of analysis

What was hard:
- Interpreting the huge gap between current_stock and max_stock
- Mapping all foreign keys across 12 tables

Improvement for Sprint 2:
- Formalise cleaning rules and document join logic before analysis
- Standardise unit handling notes in the data dictionary

## Deliverables committed to GitHub
- notebooks/data_exploration.ipynb
- docs/erd-map.md
- docs/data-dictionary-draft.md
- docs/data-quality-findings.md
- docs/kpi_hypotheses.md
- sprint-notes.md
