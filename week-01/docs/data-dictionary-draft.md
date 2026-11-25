# Draft Data Dictionary | Sprint 1
Based on CadetX official Fields Documentation.docx

| Table | Field | Type | Description | Notes |
|---|---|---|---|---|
| branches.csv | branch_id | int | Unique warehouse/branch ID | Primary key |
| branches.csv | branch_name | string | Warehouse name | |
| branches.csv | location | string | Geographic location | |
| customers.csv | customer_id | int | Unique customer ID | PK |
| customers.csv | customer_name | string | Customer name | |
| inventory_master.csv | inventory_id | int | Inventory snapshot ID | PK |
| inventory_master.csv | product_id | int | Product ID | FK to products |
| inventory_master.csv | branch_id | int | Warehouse ID | FK to branches |
| inventory_master.csv | current_stock | float | Current on-hand quantity | Extreme high values observed |
| inventory_master.csv | reorder_point | float | Reorder trigger level | |
| inventory_master.csv | max_stock | float | Maximum target stock | Much smaller than current_stock |
| products.csv | product_id | int | Unique product ID | PK |
| products.csv | product_name | string | Product name | Heavy goods |
| products.csv | category | string | Product category | |
| stock_ledger.csv | ledger_id | int | Stock transaction ID | PK |
| stock_ledger.csv | product_id | int | Product ID | FK |
| stock_ledger.csv | branch_id | int | Warehouse ID | FK |
| stock_ledger.csv | transaction_date | date | Date of stock movement | 2019–2025 |
| stock_ledger.csv | qty_change | float | +/- quantity | +receive, -issue |
| stock_ledger.csv | transaction_type | string | Movement type | Sale / Purchase / Adjustment |
| suppliers.csv | supplier_id | int | Supplier ID | PK |
| suppliers.csv | supplier_name | string | Supplier name | |
| sales_orders_header.csv | sales_order_id | int | Sales order ID | PK |
| sales_orders_header.csv | customer_id | int | Customer ID | FK |
| sales_orders_header.csv | order_date | date | Order date | |
| sales_orders_lines.csv | sales_line_id | int | Line item ID | PK |
| sales_orders_lines.csv | sales_order_id | int | Parent sales order | FK |
| sales_orders_lines.csv | product_id | int | Product sold | FK |
| sales_orders_lines.csv | quantity | float | Sold quantity | |
| purchase_orders_header.csv | po_id | int | Purchase order ID | PK |
| purchase_orders_header.csv | supplier_id | int | Supplier ID | FK |
| purchase_orders_header.csv | order_date | date | PO raised date | |
| purchase_orders_lines.csv | po_line_id | int | PO line ID | PK |
| purchase_orders_lines.csv | po_id | int | Parent PO | FK |
| purchase_orders_lines.csv | product_id | int | Product purchased | FK |
| purchase_orders_lines.csv | quantity | float | Ordered quantity | |
| invoices.csv | invoice_id | int | Invoice ID | PK |
| invoices.csv | sales_order_id | int | Linked sales order | FK |
| payments.csv | payment_id | int | Payment ID | PK |
| payments.csv | invoice_id | int | Linked invoice | FK |
