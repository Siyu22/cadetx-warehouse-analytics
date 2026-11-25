# ERD & Table Relationship Map
```mermaid
erDiagram
    BRANCHES {
        branch_id PK
        branch_name
        location
    }
    CUSTOMERS {
        customer_id PK
        customer_name
    }
    SUPPLIERS {
        supplier_id PK
        supplier_name
    }
    PRODUCTS {
        product_id PK
        product_name
        category
    }
    INVENTORY_MASTER {
        inventory_id PK
        product_id FK
        branch_id FK
        current_stock
        reorder_point
        max_stock
    }
    STOCK_LEDGER {
        ledger_id PK
        product_id FK
        branch_id FK
        transaction_date
        qty_change
        transaction_type
    }
    SALES_ORDERS_HEADER {
        sales_order_id PK
        customer_id FK
        order_date
    }
    SALES_ORDERS_LINES {
        sales_line_id PK
        sales_order_id FK
        product_id FK
        quantity
    }
    PURCHASE_ORDERS_HEADER {
        po_id PK
        supplier_id FK
        order_date
    }
    PURCHASE_ORDERS_LINES {
        po_line_id PK
        po_id FK
        product_id FK
        quantity
    }
    INVOICES {
        invoice_id PK
        sales_order_id FK
    }
    PAYMENTS {
        payment_id PK
        invoice_id FK
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
