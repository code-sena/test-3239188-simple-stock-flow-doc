# Features and Value — Simple Stock Flow

## 1. Core Features
* **Catalog Management:** Secure registration and tracking of products grouped by categories.
* **Controlled Inventory Tracking:** Real-time stock updates protected by strict database invariants (`stock >= 0`).
* **Immutable Sales Processing:** Recording completed sales with locked-in prices and quantities to ensure permanent, reliable financial auditing.

## 2. Business Value
* **Elimination of Stock Anomalies:** Prevents negative inventory scenarios through database-level enforcement.
* **Audit Readiness:** Maintains clean, immutable historical records of all sale transactions.
* **Maintainable Architecture:** Built on Hexagonal Architecture and Domain-Driven Design principles, ensuring long-term code quality and adaptability for future extensions.
