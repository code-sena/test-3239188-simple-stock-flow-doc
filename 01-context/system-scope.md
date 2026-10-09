# System Scope — Simple Stock Flow

## 1. In-Scope (Functional Boundaries)
* **Inventory & Catalog Management:** Registration and updating of products and categories.
* **Stock Enforcement:** Guaranteeing that stock levels cannot drop below zero (`stock >= 0`) via database constraints.
* **Sales Recording:** Capturing immutable sales transactions and line items with frozen pricing.
* **Authentication & Roles:** Managing internal operators (`admin` and `seller`).

## 2. Out-of-Scope
* **External Payment Gateways:** Direct integration with third-party payment processors or credit card clearinghouses (simulated or handled externally).
* **Multi-Warehouse Logistics:** Complex cross-location supply chain management (the system focuses on a unified stock flow model).
* **Customer Portal:** External customer-facing shopping interfaces (operators record transactions on behalf of the business).
