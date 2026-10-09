# Requirements — Simple Stock Flow

## 1. Purpose
This document compiles functional requirements derived from the data model and quality requirements that can be supported by it. It does not replace an API contract or a full acceptance specification.

**Base source:** `data-model.md`, §§1–§7.

## 2. Functional Requirements

### RF-01. Query Categories
The system must allow reading the five predefined categories. It must not offer operations to create, modify, or delete categories through the domain repository.

**Source:** §2.1 and §9.

### RF-02. Query and Search Products
The system must allow searching for products by partial text and category, listing active products, and querying a product by identifier.

**Source:** §6.1, patterns Q1–Q3.

### RF-03. Maintain Product Data
A product contains name, price, stock, category, and optional image. The name is truncated; the price must be greater than zero; the category must exist; a missing image is represented by `NULL`.

**Source:** §1 and §2.2.

### RF-04. Inventory Control
The system must allow stock withdrawals and replenishments without leaving a negative balance. Withdrawals exceeding the available units must be rejected.

**Source:** §2.2 and §4.

### RF-05. Logically Deleting Products
The system must hide deleted products using the `deleted_at` mechanism and the global filter; it must not physically delete them as a normal operation.

**Source:** §2.2 and §3.

### RF-06. Recording Sales
The system must record the time of the sale and the user making it. A confirmable sale must have at least one line.

**Source:** §1 and §2.3.

### RF-07. Register sales lines
Each line must correspond to a product, have a positive quantity, and retain the name, price, and category name that correspond to the time of the sale. A product should not be repeated in two lines of the same sale.

**Source:** §2.3–§2.4.

### RF-08. Maintain consistency between sales and stock
Adding a sales line and deducting the corresponding quantity from stock should be treated as a single domain operation.

**Source:** §2.3.

### RF-09. Maintain the immutability of sales
A registered sale should not be editable or deletable through domain ports.

**Source:** §1 and §2.3.

### RF-10. Query sales by period
The system must allow querying sales within a date range. The range cannot have an end date earlier than the start date.

**Source:** Sections 1 and 6.1, Q7–Q8.

### RF-11. Generate Aggregate Report
The system must calculate an aggregate report per product for a date range. The report is not stored in a separate table; it is calculated in the engine using a read port.

**Source:** Sections 1 and 6.1, Q9.

### RF-12. Authenticate Internal Operators
The system must identify internal users using their username and password hash. The domain must not receive the password in plain text. Valid roles are `admin` and `seller`.

**Source:** Sections 1 and 2.5.

## 3. Non-Functional/Quality Requirements

### RNF-01. Stock Integrity
PostgreSQL must prevent `product.stock` from being negative using `ck_product_stock_non_negative`.


**Source:** §2.2 and §4.

### RNF-02. Referential Integrity
A product category must exist. Lines must belong to a sale and reference a product; the sale must be attributed to an existing user. Relationships listed as pending must be implemented before referential integrity is considered complete.

**Source:** §4–§5.

### RNF-03. Credential Protection
Only the `password_hash` is stored; it must not be indexed or replaced by plaintext passwords. The domain must not access the original key.

**Source:** §1, §2.5, and §7.

### RNF-04. Historical Preservation
The name and price saved in a sales line must not change when the product is updated in the catalog. The historical category name must also not depend on a foreign key to the current category.

**Source:** §1, §2.4, and §5.

### RNF-05. Monetary Precision
Amounts must retain two decimal places, and the domain must round using `MidpointRounding.AwayFromZero`. The price column uses `numeric(18,2)`.

**Source:** §2.2 and §3.

### RNF-06. Date Consistency
Timestamps use `timestamptz`; the verified server operates in UTC. `sold_at` is the only business instant identified in the model.

**Source:** §3 and §8.

### RNF-07. Schema Management
DDL changes must be performed through Entity Framework migrations; a second mechanism that independently modifies the schema must not be maintained.

**Source:** §3.2.


### RNF-08. Query Performance
Indexes must be justified by documented access patterns. The model identifies three pending indexes in T-13 and other existing indexes; pending indexes should not be presented as implemented.

**Source:** §6.1–§6.3.

## 4. Undefined requirements and details
The model does not establish:
- maximum response times or target attendance;
- percentage availability;
- specific authorization rules for `admin` vs `seller`;
- screens, navigation flows or interface messages;
- length limits for passwords entered by the user;
- complete API acceptance criteria.

These points remain pending specification and numerical values ​​are not invented.
