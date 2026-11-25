# ERD & Table Relationship Map
```mermaid
erDiagram
    BRANCHES {
        int branch_id PK
        string branch_name
        string location
    }
    CUSTOMERS {
        int customer_id PK
        string customer_name
    }
    SUPPLIERS {
        int supplier_id PK
        string supplier_name
    }
    PRODUCTS {
        int product_id PK
        string product_name
        string category
    }
    INVENTORY_MASTER {
        int inventory_id PK
        int product_id FK
        int branch_id FK
        float current_stock
        float reorder_point
        float max_stock
    }
    STOCK_LEDGER {
        int ledger_id PK
        int product_id FK
        int branch_id FK
        date transaction_date
        float qty_change
        string transaction_type
    }
    SALES_ORDERS_HEADER {
        int sales_order_id PK
        int customer_id FK
        date order_date
    }
    SALES_ORDERS_LINES {
        int sales_line_id PK
        int sales_order_id FK
        int product_id FK
        float quantity
    }
    PURCHASE_ORDERS_HEADER {
        int po_id PK
        int supplier_id FK
        date order_date
    }
    PURCHASE_ORDERS_LINES {
        int po_line_id PK
        int po_id FK
        int product_id FK
        float quantity
    }
    INVOICES {
        int invoice_id PK
        int sales_order_id FK
    }
    PAYMENTS {
        int payment_id PK
        int invoice_id FK
    }

    BRANCHES ||--o{ INVENTORY_MASTER : "holds"
    BRANCHES ||--o{ STOCK_LEDGER : "records movement"
    CUSTOMERS ||--o{ SALES_ORDERS_HEADER : "places"
    SUPPLIERS ||--o{ PURCHASE_ORDERS_HEADER : "fulfills"
    PRODUCTS ||--o{ INVENTORY_MASTER : "stocked"
    PRODUCTS ||--o{ STOCK_LEDGER : "tracked"
    PRODUCTS ||--o{ SALES_ORDERS_LINES : "sold"
    PRODUCTS ||--o{ PURCHASE_ORDERS_LINES : "purchased"
    SALES_ORDERS_HEADER ||--o{ SALES_ORDERS_LINES : "contains"
    PURCHASE_ORDERS_HEADER ||--o{ PURCHASE_ORDERS_LINES : "contains"
    SALES_ORDERS_HEADER ||--o{ INVOICES : "invoiced"
    INVOICES ||--o{ PAYMENTS : "paid"

