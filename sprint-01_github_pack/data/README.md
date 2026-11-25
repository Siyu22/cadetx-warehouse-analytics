# Data

Raw dataset supplied by the programme (not committed to the repository).

Download: https://www.cadetx.co.uk/datasets/HeavySuppliersWarehouseDatasets.zip

Extract the 12 CSV files into this folder:

```
data/
├── branches.csv
├── customers.csv
├── inventory_master.csv
├── invoices.csv
├── payments.csv
├── products.csv
├── purchase_orders_header.csv
├── purchase_orders_lines.csv
├── sales_orders_header.csv
├── sales_orders_lines.csv
├── stock_ledger.csv
└── suppliers.csv
```

Row counts at Sprint 1:

| Table | Rows | Columns |
|---|---|---|
| branches.csv | 6 | 13 |
| customers.csv | 500 | 15 |
| inventory_master.csv | 180 | 8 |
| invoices.csv | 18,033 | 10 |
| payments.csv | 19,257 | 5 |
| products.csv | 30 | 23 |
| purchase_orders_header.csv | 24,000 | 11 |
| purchase_orders_lines.csv | 155,495 | 9 |
| sales_orders_header.csv | 20,000 | 11 |
| sales_orders_lines.csv | 130,402 | 9 |
| stock_ledger.csv | 237,230 | 9 |
| suppliers.csv | 8 | 12 |

The notebook in `week-01/notebooks/` reads from `../../data` by default.
Override with an environment variable if the layout differs:

```bash
DATA_DIR=/path/to/csvs jupyter nbconvert --execute week-01/notebooks/data_exploration.ipynb
```
