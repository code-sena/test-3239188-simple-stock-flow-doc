# Domain Entities and Business Rules — Simple Stock Flow

## 1. Core Domain Entities

### Product (Aggregate Root)
* **Attributes:** `id` (UUID), `sku` (String, Unique), `name` (String), `description` (String), `price` (Decimal), `categoryId` (UUID), `isActive` (Boolean).
* **Business Invariants:**
  * The price must be strictly greater than `0`.
  * The SKU must be unique across the entire organization and follow standard naming conventions.
  * An inactive product cannot be associated with new sales transactions.

### StockMovement
* **Attributes:** `id` (UUID), `productId` (UUID), `quantity` (Integer), `type` (Enum: `IN`, `OUT`, `ADJUSTMENT`), `timestamp` (DateTime), `userId` (UUID).
* **Business Invariants:**
  * Available stock can never become negative as a result of an `OUT` movement.
  * Every stock modification must be explicitly linked to an authenticated user and record an immutable UTC timestamp.

### Sale
* **Attributes:** `id` (UUID), `userId` (UUID), `totalAmount` (Decimal), `status` (Enum: `PENDING`, `COMPLETED`, `CANCELLED`), `createdAt` (DateTime).
* **Business Invariants:**
  * A completed sale locks product prices at the exact moment of checkout.
  * If a sale is cancelled, associated units must automatically return to inventory via a compensating reversal movement.

## 2. Cross-Cutting Validation Rules
* Operations involving inventory balance modifications must execute within atomic database transactions (ACID compliant) to prevent race conditions during concurrent sales.
