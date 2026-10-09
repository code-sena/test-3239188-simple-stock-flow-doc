# Domain Model — Simple Stock Flow

## 1. Entities and Responsibilities

### 1.1 `Category` — Reference Category
Represents a fixed product classification. There are five categories seeded since the initial migration. The domain requires that the name be mandatory, non-empty, and truncated; the engine guarantees name uniqueness. Its repository is read-only: categories are not created, renamed, or deleted from the application.

**Rule Status:** Non-empty and truncated name: domain only; uniqueness: engine. The name restriction in the engine is pending in T-20.

**Source:** `data-model.md`, §1, §2.1, and §4.

### 1.2 `Product` — Aggregate Root
Represents a product in the catalog. Its only business attributes are name, price, stock, category, and optional image.

Rules:
- The name is mandatory, non-empty, and truncated before saving.
- The price must be strictly greater than zero.

- Stock can never be negative.

- Removing more units than are available is not allowed.

- The category is mandatory and must exist.

- A missing image is represented by `NULL`, not an empty string.

- Deletion is logical using `deleted_at`; the product is not physically deleted.

`Money` is rounded to two decimal places using `MidpointRounding.AwayFromZero`.

**Rule status:** `stock >= 0` and the relationship with category are protected by the engine; `price > 0`, non-empty name, and image normalization depend on the domain. The price check is pending in T-20.

**Source:** `data-model.md`, §1, §2.2, §3, and §4.

### 1.3 `Sale` — Aggregate Root
Represents a completed and immutable sale, including the operator who recorded it, the time, and its lines.

Rules:
- Must record who made the sale.

- Must contain at least one line to be confirmed.

- The same product cannot appear twice in a sale.

- Adding a line and deducting the stock are a single domain operation.

- A recorded sale cannot be edited or deleted.

- The total is calculated by summing the subtotals of its lines; it is not stored.

**Rule Status:** The presence of at least one line, immutability, and the combined stock and line operation depend on the domain. The uniqueness of `(sale_id, product_id)` is described as existing since T-20, but `sale_id` being nullable is a defect that must be corrected.

**Source:** `data-model.md`, §1, §2.3, §3, and §4.

### 1.4 `SaleItem` — Internal entity of `Sale`
Represents a sales line with product, quantity, and data frozen upon sale.

Rules:
- It is only created from `Sale.AddItem`.

- The product is required.

- The quantity must be greater than zero.

- The name and price are copied from the product at the time of sale.

- The category name is retained as historical data, without a foreign key to `category`.

- The line must not exist without its sale.

**Rule status:** The positive quantity depends on the domain; its `CHECK` is pending in T-20. The product foreign key is declared pending implementation in T-20. `sale_item.sale_id` appears nullable in the described schema and should be set to `NOT NULL`; the model identifies it as a default.

**Source:** `data-model.md`, §1, §2.4, §3, §4, and §5.

### 1.5 `User` — Aggregate Root
Represents an internal operator who authenticates and logs sales.

Rules:
- The username is required and unique.

- The username is normalized to lowercase and truncated.

- The password hash is required and cannot be empty.

- The role can only be `admin` or `seller`.

- The domain does not receive or store the password in plain text; the hash is generated via a port.

**Rule Status:** Uniqueness and `NOT NULL` are protected by the engine; Normalization and role validation depend on the domain. Equivalent engine constraints are pending in T-20.

**Source:** `data-model.md`, §1, §2.5, and §4.

## 2. Relationships
- `Category` 1:N `Product`: Each product belongs to a mandatory category.

- `Sale` 1:N `SaleItem`: A sale is composed of its lines.

- `Product` 1:N `SaleItem`: A product can appear on different sale lines over time.

- `User` 1:N `Sale`: A user can register multiple sales.

`Sale` and `Product` maintain an N:M relationship resolved by `SaleItem`.

The `Sale`–`User` relationship has a planned but pending foreign key (FK-4/T-12). The `SaleItem`–`Product` relationship is also documented as a planned foreign key; its implementation is associated with T-20.

**Source:** `data-model.md`, §5.

## 3. Value Objects and Calculations
- **`Money`:** Monetary value with two decimal places; the domain rounds using `MidpointRounding.AwayFromZero`.

- **`Quantity`:** Positive quantity for a sales line.

- **Date Range:** The end date cannot be earlier than the start date.

- **Subtotal:** Fixed unit price × quantity.

- **Sales Total:** Sum of subtotals.

- **Sales Report:** Aggregation by product for a date range; calculated in PostgreSQL using a read port and is not persisted.

**Source:** `data-model.md`, §1, §2.2, §2.3, and §6.

## 4. Domain Events
The data model does not specify domain event names or confirm that events are published. Therefore, technical events are not created.

The following are identifiable business facts—not implemented events:
- product created or updated;

- stock replenished or withdrawn;

- sale recorded;

- line added to a sale.

**Status:** Names, publication, and handling of technical events are pending definition.

**Source:** Limited inference from the operations described in `data-model.md`, §2.2–§2.4.

## 5. Glossary

| Term | Definition |
|------|------------|
| Product | Item with name, price, stock, category, and optional image. |
| Category | One of five fixed classifications. |
| Stock | Available units; can never be negative. |
| Sale | Recorded and immutable business fact. |
| Sales Line | Product, quantity, and historical sales data. |
| Frozen Price | Price copied when the sale is recorded; does not change if the catalog changes. |
| Low Logic | Mark a product as deleted without physically deleting it. |
Password Hash | Irreversible fingerprint; the domain does not see the password in plaintext. |
Role | One of the values ​​`admin` or `seller`. |
Aggregate Report | Calculated result per product for a date range. |

**Source:** `data-model.md`, §1–§2.5.
