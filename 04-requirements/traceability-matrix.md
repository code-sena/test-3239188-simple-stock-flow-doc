# Traceability Matrix — Simple Stock Flow

| User Story | Functional Requirement | Technical Enforcement Location | Enforcement Type |
| :--- | :--- | :--- | :--- |
| US-01 | FR-01 | `product` table / `Product` entity | `motor` (PostgreSQL constraints) |
| US-02 | FR-03 | `product.stock` check constraint | `motor` (Database level) |
| US-03 | FR-02 | `sale` & `sale_item` tables | `motor` & domain logic |
