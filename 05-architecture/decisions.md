# Architectural Decisions — Simple Stock Flow

This document compiles technical and structural decisions of the system (aligned with project ADRs).

* **D-01 to D-10 / ADR-001 (Database Structure):** Use of the `sales` schema in PostgreSQL 16 with singular table names (`product`, `category`, `sale`, `sale_item`, `user`) to maintain strict coherence with governance conventions.
* **ADR-002 (Optimistic Concurrency):** Stock can never be negative (`stock >= 0`). Validation is delegated and enforced at the database engine level (`motor`) via native constraints, ensuring external writes (via psql or other clients) respect the invariant.
* **Mapping & Conventions:** C# entities use singular names aligned with tables, while ORM collections are managed in plural within Entity Framework contexts.
