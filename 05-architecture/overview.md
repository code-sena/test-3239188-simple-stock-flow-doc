# Architectural Overview — Simple Stock Flow

## 1. Architectural Style
The system is built upon the principles of **Hexagonal Architecture (Ports and Adapters)** and **Domain-Driven Design (DDD)**. 

The fundamental purpose of this approach is to protect the business core (`Domain`) from any external technological details, ensuring that the PostgreSQL database, web frameworks, or infrastructure adapters depend on the domain and never vice versa.

## 2. Layer Structure
* **Domain Core (`Domain`):** Contains pure entities (`Product`, `Sale`, `SaleItem`, `User`, `Category`), value objects (`Money`, `Quantity`), and non-negotiable business invariants.
* **Application Layer (`Application`):** Orchestrates system use cases through driving ports and defines outbound ports (repository interfaces) needed to persist or query data.
* **Infrastructure Layer (`Infrastructure`):** Implements output adapters (such as Entity Framework Core interacting with PostgreSQL under the `sales` schema) and input adapters (REST API).
