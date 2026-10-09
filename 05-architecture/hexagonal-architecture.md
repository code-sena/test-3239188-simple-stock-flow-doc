# Hexagonal Architecture (Ports and Adapters) — Simple Stock Flow

## 1. Architectural Layers & Separation of Concerns
The project strictly enforces Hexagonal Architecture to isolate core business logic from external frameworks, databases, and user interfaces:

* **Core Domain (`Domain`):** Contains pure entities, value objects, domain events, and business rules. It has zero external dependencies on frameworks, ORMs, or databases.
* **Application Services (`Application`):** Orchestrates use cases and system workflows by applying domain rules without containing business logic itself.
* **Ports (`Ports`):** 
  * *Inbound Ports (Driving):* Interfaces defining use cases exposed to external drivers (e.g., REST controllers).
  * *Outbound Ports (Driven):* Interfaces defining contracts for external systems (e.g., repository interfaces for database persistence).
* **Adapters (`Adapters`):** 
  * *Inbound Adapters:* REST API controllers, CLI commands, or web handlers.
  * *Outbound Adapters:* PostgreSQL database implementations, JPA/Hibernate entities, or external logging tools.
