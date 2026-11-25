# Data Dictionary (Draft) — Sprint 1

Auto-generated from the 12 source CSVs, then annotated by the team.
The full descriptive dictionary is a Sprint 2–3 deliverable; this draft fixes field names, types and known caveats
so every later analysis uses the same vocabulary.

**Coverage:** 12 tables · 134 fields · 605,141 rows

| Table | Field | Type | Meaning | Sample | Caveat |
|---|---|---|---|---|---|
| branches | branch_id | text | Branch id | DEL001 | — |
| branches | branch_name | text | Branch name | Delhi Central | — |
| branches | city | text | City | Delhi | — |
| branches | state | text | State | Delhi | — |
| branches | region | text | Region | North | — |
| branches | warehouse_type | text | Warehouse type | Central | — |
| branches | warehouse_capacity | text | Warehouse capacity | 45230 sqft | Text with unit suffix (e.g. '45230 sqft') — parse before calculating (DQ-07). |
| branches | service_center_available | text | Service center available | Yes | — |
| branches | manager_id | integer | Manager id | 9593 | — |
| branches | total_employees | integer | Total employees | 92 | — |
| branches | avg_monthly_revenue | integer | Avg monthly revenue | 5234890 | — |
| branches | monthly_operational_cost | integer | Monthly operational cost | 3189000 | — |
| branches | market_demand_index | integer | Market demand index | 9 | — |
| customers | customer_id | text | Customer id | C0001 | Text identifier (C0001 style). |
| customers | customer_type | text | Customer type | Retail | — |
| customers | industry_segment | text | Industry segment | Construction | — |
| customers | city | text | City | Kolkata | — |
| customers | state | text | State | West Bengal | — |
| customers | pincode | text | Pincode | 991035 | Keep as text to preserve leading zeros. |
| customers | region | text | Region | East | — |
| customers | branch_id | text | Branch id | KOL001 | — |
| customers | credit_limit | integer | Credit limit | 45896 | — |
| customers | current_balance | integer | Current balance | 43284 | — |
| customers | payment_terms | text | Payment terms | Advance | — |
| customers | customer_since | date | Customer since | 2024-04-10 | — |
| customers | last_purchase_date | date | Last purchase date | 2024-04-25 | — |
| customers | total_purchase_value | integer | Total purchase value | 3892818 | — |
| customers | customer_rating | integer | Customer rating | 4 | — |
| inventory_master | product_id | text | Product id | P001 | Text identifier (P001 style). |
| inventory_master | branch_id | text | Branch id | DEL001 | — |
| inventory_master | opening_stock | integer | Opening stock | 196 | Two orders of magnitude below current_stock; snapshot scale mismatch. |
| inventory_master | reorder_level | integer | Reorder level | 59 | Also defined at product level in products.csv — two conflicting thresholds (DQ-06). |
| inventory_master | safety_stock | integer | Safety stock | 46 | Also defined at product level in products.csv. |
| inventory_master | max_stock | integer | Max stock | 340 | Configured upper bound, never recalibrated to actual volume. |
| inventory_master | current_stock | integer | Current stock | 115928 | Matches stock_ledger closing balance exactly, but ~349× the configured max_stock — see DQ-01. |
| inventory_master | warehouse_bin | text | Warehouse bin | C15 | — |
| products | product_id | text | Product id | P001 | Text identifier (P001 style). |
| products | product_name | text | Product name | Hydraulic Pump HP-300 | — |
| products | category | text | Category | Hydraulic | — |
| products | machine_type | text | Machine type | Excavator | — |
| products | brand | text | Brand | CAT | — |
| products | model_compatibility | text | Model compatibility | CAT 320D | — |
| products | unit_cost | integer | Unit cost | 18950 | Used for inventory valuation and COGS. |
| products | unit_price | integer | Unit price | 24500 | — |
| products | margin_percentage | decimal | Margin percentage | 29.2 | Static margin on list price, not realised margin. |
| products | gst_rate | integer | Gst rate | 28 | Indian GST rate. |
| products | weight_kg | decimal | Weight kg | 38.5 | — |
| products | dimensions_cm | text | Dimensions cm | 40x25x25 | — |
| products | material_type | text | Material type | steel | — |
| products | warranty_months | integer | Warranty months | 12 | — |
| products | reorder_level | integer | Reorder level | 15 | Also defined at product level in products.csv — two conflicting thresholds (DQ-06). |
| products | safety_stock | integer | Safety stock | 10 | Also defined at product level in products.csv. |
| products | max_stock_level | integer | Max stock level | 40 | Product-level version of max_stock; conflicts with inventory_master.max_stock. |
| products | lead_time_days | integer | Lead time days | 21 | Promised lead time; compared with actual (received_date - order_date). |
| products | criticality_level | text | Criticality level | High | — |
| products | usage_frequency | text | Usage frequency | Medium | — |
| products | uom | text | Uom | piece | — |
| products | last_purchase_price | integer | Last purchase price | 18500 | — |
| products | last_purchase_date | date | Last purchase date | 2019-03-15 | — |
| suppliers | supplier_id | text | Supplier id | SUP0001 | — |
| suppliers | supplier_name | text | Supplier name | Shenzhen OEM Supplies | — |
| suppliers | supplier_type | text | Supplier type | OEM | — |
| suppliers | product_category | text | Product category | Medium Parts | — |
| suppliers | city | text | City | Shenzhen | — |
| suppliers | province | text | Province | Guangdong | — |
| suppliers | region | text | Region | South China | — |
| suppliers | pincode | text | Pincode | 586808 | Keep as text to preserve leading zeros. |
| suppliers | lead_time_days | integer | Lead time days | 19 | Promised lead time; compared with actual (received_date - order_date). |
| suppliers | reliability_score | integer | Reliability score | 5 | 1–5 score not correlated with measured on-time delivery (DQ-05). |
| suppliers | import_duty_rate | integer | Import duty rate | 8 | — |
| suppliers | china_tax_id | text | China tax id | 90SUZHOU398241 | — |
| sales_orders_header | so_id | text | So id | SO-990591 | — |
| sales_orders_header | customer_id | text | Customer id | C0070 | Text identifier (C0001 style). |
| sales_orders_header | branch_id | text | Branch id | AHM001 | — |
| sales_orders_header | order_date | date | Order date | 2022-05-24 | — |
| sales_orders_header | delivery_date | date | Delivery date | 2022-05-28 | — |
| sales_orders_header | order_status | text | Order status | Delivered | — |
| sales_orders_header | payment_terms | text | Payment terms | Net 30 | — |
| sales_orders_header | total_order_value | integer | Total order value | 1345720 | Revenue definition: excl. GST, delivered orders only. |
| sales_orders_header | total_gst_amount | decimal | Total gst amount | 374439.60000000003 | — |
| sales_orders_header | grand_total | decimal | Grand total | 1720159.6 | Incl. GST. |
| sales_orders_header | sales_channel | text | Sales channel | Counter Sale | — |
| sales_orders_lines | so_id | text | So id | SO-990591 | — |
| sales_orders_lines | line_number | integer | Line number | 1 | — |
| sales_orders_lines | product_id | text | Product id | P018 | Text identifier (P001 style). |
| sales_orders_lines | quantity | integer | Quantity | 19 | Pieces. Purchase quantity runs ~18× ahead of sales quantity (DQ-02). |
| sales_orders_lines | unit_price | integer | Unit price | 11300 | — |
| sales_orders_lines | gst_rate | integer | Gst rate | 28 | Indian GST rate. |
| sales_orders_lines | line_total | integer | Line total | 214700 | — |
| sales_orders_lines | gst_amount | decimal | Gst amount | 60116.00000000001 | — |
| sales_orders_lines | line_grand_total | decimal | Line grand total | 274816.0 | — |
| purchase_orders_header | po_id | text | Po id | PO-141186 | — |
| purchase_orders_header | supplier_id | text | Supplier id | SUP0001 | — |
| purchase_orders_header | branch_id | text | Branch id | PUN001 | — |
| purchase_orders_header | order_date | date | Order date | 2019-04-10 | — |
| purchase_orders_header | expected_delivery_date | text | Expected delivery date | 2019-04-29 | — |
| purchase_orders_header | received_date | date | Received date | 2019-04-29 | Null for all 2,370 cancelled POs — status-consistent, not a missing-value defect. |
| purchase_orders_header | po_status | text | Po status | Received | — |
| purchase_orders_header | total_cost | decimal | Total cost | 22955232.11 | — |
| purchase_orders_header | total_gst_amount | decimal | Total gst amount | 6427464.990800001 | — |
| purchase_orders_header | grand_total | decimal | Grand total | 29382697.1008 | Incl. GST. |
| purchase_orders_lines | po_id | text | Po id | PO-141186 | — |
| purchase_orders_lines | line_number | integer | Line number | 1 | — |
| purchase_orders_lines | product_id | text | Product id | P004 | Text identifier (P001 style). |
| purchase_orders_lines | quantity | integer | Quantity | 234 | Pieces. Purchase quantity runs ~18× ahead of sales quantity (DQ-02). |
| purchase_orders_lines | unit_cost | decimal | Unit cost | 34709.54 | Used for inventory valuation and COGS. |
| purchase_orders_lines | gst_rate | integer | Gst rate | 28 | Indian GST rate. |
| purchase_orders_lines | line_total | decimal | Line total | 8122032.36 | — |
| purchase_orders_lines | gst_amount | decimal | Gst amount | 2274169.0608 | — |
| purchase_orders_lines | line_grand_total | decimal | Line grand total | 10396201.4208 | — |
| invoices | invoice_id | text | Invoice id | INV-325361 | 196 IDs appear twice with different sales orders (DQ-03). |
| invoices | so_id | text | So id | SO-990591 | — |
| invoices | customer_id | text | Customer id | C0070 | Text identifier (C0001 style). |
| invoices | branch_id | text | Branch id | AHM001 | — |
| invoices | invoice_date | date | Invoice date | 2022-05-28 | — |
| invoices | due_date | date | Due date | 2022-06-27 | — |
| invoices | total_order_value | integer | Total order value | 1345720 | Revenue definition: excl. GST, delivered orders only. |
| invoices | total_gst_amount | decimal | Total gst amount | 374439.6 | — |
| invoices | grand_total | decimal | Grand total | 1720159.6 | Incl. GST. |
| invoices | payment_status | text | Payment status | Unpaid | 155 invoices are overpaid and 17 marked Unpaid have payments (DQ-04). |
| payments | payment_id | text | Payment id | PAY-207775 | — |
| payments | invoice_id | text | Invoice id | INV-786913 | 196 IDs appear twice with different sales orders (DQ-03). |
| payments | payment_date | date | Payment date | 2023-10-07 | — |
| payments | payment_amount | decimal | Payment amount | 3354624.0 | — |
| payments | payment_method | text | Payment method | Cheque | — |
| stock_ledger | movement_id | text | Movement id | MOV-OUT-184742 | — |
| stock_ledger | product_id | text | Product id | P001 | Text identifier (P001 style). |
| stock_ledger | branch_id | text | Branch id | AHM001 | — |
| stock_ledger | movement_type | text | Movement type | OUT | — |
| stock_ledger | movement_date | date | Movement date | 2019-01-03 | — |
| stock_ledger | quantity | integer | Quantity | 8 | Pieces. Purchase quantity runs ~18× ahead of sales quantity (DQ-02). |
| stock_ledger | reference_type | text | Reference type | SO | — |
| stock_ledger | reference_id | text | Reference id | SO-157767 | — |
| stock_ledger | running_balance | integer | Running balance | 228 | Rolling balance per product × branch; no negative values. |
