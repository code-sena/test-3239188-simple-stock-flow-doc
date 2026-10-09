# Domain Context — Simple Stock Flow

## 1. Business Purpose and Scope
The **Simple Stock Flow** system is designed to automate and control inventory flow, stock levels, and commercial transactions for small to medium-scale operations. Its primary purpose is to eliminate inventory discrepancies through real-time logging of goods intake and outflow, ensuring end-to-end traceability of commercial operations and financial security.

## 2. Bounded Contexts (DDD)
The domain is strictly separated into three core bounded contexts to maintain loose coupling:
* **Catalog Context:** Manages product identity, physical attributes, categorization, pricing rules, and units of measurement.
* **Inventory (Stock) Context:** Handles physical and virtual stock levels, minimum/maximum safety stock thresholds, warehouse locations, and movement audit trails.
* **Transactional (Sales) Context:** Processes sales orders, validates real-time stock availability during checkout, manages payment states, and logs customer financial transactions.

## 3. Ubiquitous Language & Core Actors
* **Inventory Administrator:** Responsible for catalog lifecycle management, category structuring, and discrepancy auditing.
* **Cashier / Sales Operator:** Executes sales transactions, triggering automated stock deductions in active inventory.
* **System Auditor / Manager:** Reviews historical movement logs and performance metrics for business forecasting.
