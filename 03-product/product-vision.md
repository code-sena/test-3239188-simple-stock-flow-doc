# Product Vision — Simple Stock Flow

## 1. Vision
Simple Stock Flow aims to provide a controlled way to manage products and inventory, record sales by internal operators, and view sales results by product over a period.

This vision is derived from the entities, rules, and access patterns described in the data model; it does not replace a complete functional specification.

**Source:** `data-model.md`, §1, §2, and §6.

## 2. Need it addresses
The system must maintain consistency in three sets of information:
1. The product catalog, including categories, prices, and stock levels.

2. Recorded sales, including product lines and historical data at the time of sale.

3. Internal users who record sales and their roles.

**Source:** `data-model.md`, §1–§5.

## 3. Target Users
- **`admin`:** role recognized by the model.

- **`seller`:** role recognized by the model.

The model does not specify a permissions matrix per role. It is not assumed that each role has specific, exclusive operations until that rule is documented.

**Source:** `data-model.md`, §1 and §2.5.

## 4. Expected Value
- Avoid negative stock levels.

- Avoid sales with no lines and lines with non-positive quantities.

- Maintain historical sales information even if the product name or price changes in the catalog.

- Consult the aggregated report without storing copies of reports that could become outdated.

- Reduce the risk of orphaned data through foreign keys and integrity constraints.

These are objectives derived from the invariants and decisions described, not quantified metrics.

**Source:** `data-model.md`, §1, §2, §4–§6.

## 5. Product Scope
Includes catalog, fixed categories, stock management, sales registration and query, operator authentication, and aggregated reports by product and date range.

Does not include customers/buyers, additional product attributes such as SKU or description, currency per registration, sales editing or deletion, or report persistence.

**Source:** `data-model.md`, §1, §2, §6, and §12.

## 6. Success Indicators
The model does not establish numerical targets for performance, availability, or satisfaction. To verify the data-level view, observable criteria are proposed:
- Negative stock is not acceptable;

- Valid lines have a positive quantity and are associated with an existing sale and product;

- A sale retains the historical name and price of its products;

The report is calculated for the requested range and grouped by product.

These criteria are verifiable using the model's rules. Percentages and times not defined in the source are not assigned.

**Source:** `data-model.md`, §2–§6.
