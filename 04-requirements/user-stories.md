# User Stories — Simple Stock Flow

## 1. System Actors & Roles
* **Admin / Seller (`user`):** Internal operators who authenticate into the system with specific roles (`admin` or `seller`) to manage inventory, catalog items, and process sales.
* **Customer:** External context implied by sales records (no direct client entity table exists, focusing strictly on transaction execution).

## 2. Core User Stories
* **US-01: Product Catalog Management**  
  *As an* admin, *I want to* register and update products in the catalog *so that* inventory stock and pricing remain accurate.
* **US-02: Inventory Stock Control**  
  *As an* operator, *I want to* ensure product stock levels never drop below zero *so that* physical inventory matches digital records.
* **US-03: Sales Execution**  
  *As a* seller, *I want to* record immutable sales transactions with frozen item prices and quantities *so that* commercial history is accurately preserved.
