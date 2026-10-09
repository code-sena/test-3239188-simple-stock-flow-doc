# System Context — Simple Stock Flow

## 1. Purpose
Simple Stock Flow manages a product catalog and its inventory levels, records sales made by internal users, and allows users to view an aggregated sales report by product within a date range.

**Source:** `data-model.md`, §1, §2.2–§2.5 and §6.1.

## 2. Included Scope
- View the five predefined categories.

- View and search for active products, including filters by category and partial text.

- Register and view products, their price, stock, category, and optional image.

- Remove and replenish stock, ensuring that stock levels never fall below a negative value.

- Record sales with one or more lines.

- Retain the name, price, and category name that corresponded to the time of sale for each line.

- View sales by date range.

- Generate an aggregated report by product for a date range.
- Authenticate internal operators with username, password hash, and `admin` or `seller` role.

**Source:** `data-model.md`, §1, §2, §6.1, and §7.

## 3. Confirmed Out-of-Scope
- There is no customer or buyer entity.

- Description, SKU, and reference code are not included as product attributes.

- The total sale and subtotal for each line are not stored; they are calculated.

- There is no persistent report table.

- A currency column is not stored; the system works with one currency per build.

- Categories are five fixed seed data points: creating, renaming, or deleting categories through the application is not supported.

- Recorded sales are immutable: there are no editing or deletion operations.

- The image is represented with an opaque key to external storage; Binaries and file paths are not stored in `product.image_key`.

- A report broken down by vendor is not confirmed.

**Source:** `data-model.md`, §1, §2.1–§2.5, §6.3, §12.

## 4. Actors and External Systems
- **Internal Operator:** Authenticated user who records sales. The model includes the `admin` and `seller` roles.

- **External Image Storage:** External location of the binary identified by `image_key`.

- **PostgreSQL:** Persistence of system tables and calculation of the aggregated report via a read port.

**Source:** `data-model.md`, §1, §2.5, §3 and §6.

## 5. Boundaries and Integrations
The model identifies PostgreSQL 16.14, the database `simple_stock_flow`, and the schema `sales`. The tables are `category`, `product`, `sale`, `sale_item`, and `user`. The schema is managed through Entity Framework migrations; DDL should not be defined in parallel.

**Source:** `data-model.md`, header, §0 and §3.2.

## 6. Assumptions and Outstanding Issues
- **Presentation Assumption:** An interface is expected for the operator to use the above functions; the data model does not define screens or navigation.

- **Outstanding Issue:** The exact API contract, screens, and acceptance criteria are not defined by this data document.

- **Technical Outstanding Issue:** There are domain rules that do not yet have an equivalent restriction in PostgreSQL. They are listed in `05-architecture/architecture-validation.md`.

These issues are declared to avoid presenting them as already verified features.
