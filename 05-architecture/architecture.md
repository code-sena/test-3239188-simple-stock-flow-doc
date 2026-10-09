# Architecture — Simple Stock Flow

## 1. Objective
The architecture should separate the domain from persistence details and external services. Business rules are expressed in aggregates and value objects; PostgreSQL provides additional integrity constraints. The persistence adapter translates between domain classes and the relational schema.

**Source:** `data-model.md`, §0, §1–§5 and §3.2.

## 2. Domain Model and Aggregates
- **`Product`:** catalog aggregate root; controls name, price, stock, category, and image.

- **`Sale`:** sales aggregate root; coordinates lines and stock discounts.

- **`SaleItem`:** internal entity of `Sale`; should not be created outside of `Sale.AddItem`.

- **`User`:** identity and operator aggregate root.

- **`Category`:** read-only reference entity, seeded in the initial migration.

- **Value Objects:** `Money`, `Quantity`, and date range.

**Source:** §1 and §2.

## 3. Ports and Adapters
The model identifies the following architectural responsibilities:
- Product repository for searching by identifier, batch, and search criteria.

- Read-only category repository.

- Access to sales with their lines and sales query by range.

- Read port for the aggregated report by product.

- User access by username for authentication.

- Port responsible for generating the password hash, so that the domain does not receive the password in plain text.

- Persistence adapter that maps domain objects to PostgreSQL tables.

**Traceability Note:** These names describe responsibilities; The model does not provide exact interface signatures here, so no additional infrastructure method names are invented.

**Source:** §2, §6, and §7.

## 4. Persistence
The base is `simple_stock_flow`, schema `sales`, PostgreSQL 16.14. The tables are:
- `sales.category`
- `sales.product`
- `sales.sale`
- `sales.sale_item`
- `sales.user`

The tables are singular; the domain classes are also singular. C# collections can be maintained in the plural because they represent sets. The schema is modified through Entity Framework migrations.

**Source:** header, §0, §3, and §3.2.

## 5. Rules that live in the domain
Among others:
- non-empty and truncated name;

- positive price;

- Stock withdrawal cannot exceed the available stock;

- Positive quantity;

- Sale with at least one line item;

- Consistency between adding a line item and removing stock;

- Immutability of sales;

- Normalization of the username;

- Role limited to `admin` or `seller`.

These rules must be validated in the application, although some are not yet reflected as PostgreSQL constraints.

**Source:** §2 and §4.

## 6. Rules that Live in PostgreSQL
The model currently identifies primary keys, unique indexes for category and user, foreign keys from product to category, foreign keys from line item to sale, and a non-negative stock check. Other rules and relationships are described as pending at the time of the source document.

**Source:** §4–§5.

## 7. Historical Data and Reports
`SaleItem` retains the product name and price when sold, as well as the historical category name. The total and subtotals are calculated, not stored. The aggregated report by product is calculated using a read query and does not have its own table.

**Source:** §1, §2.3–§2.4, and §6.

## 8. Logical Deletion and Concurrency
Products are logically deleted using `deleted_at` and a global filter. The model identifies `xmin` as a PostgreSQL optimistic concurrency witness, exposed as a shadow property. Implementation details should follow the configuration already documented in the model.

**Source:** §2.2, §3, and §3.2.

## 9. Limitations of this Description
API paths, exact interface names, graphical interface technology, deployment, and communication contracts not defined in `data-model.md` are not to be invented. These details should remain pending if required for delivery.
