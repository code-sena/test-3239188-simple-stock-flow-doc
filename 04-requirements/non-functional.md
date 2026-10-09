# Non-Functional Requirements — Simple Stock Flow

* **NFR-01 (Data Integrity & Database Constraints):** Critical invariants—such as stock levels (`stock >= 0`)—must be enforced directly at the database engine level (`motor`) via constraints, rather than relying solely on application-layer validation.
* **NFR-02 (Naming Conventions):** All database tables must follow a singular naming convention (`product`, `category`, `sale`, `sale_item`, `user`) inside the `sales` schema. Code identifiers must be written in English.
* **NFR-03 (Concurrency & Optimistic Locking):** Concurrent stock updates must handle race conditions safely, ensuring atomic inventory decrements during high-frequency sales execution.
