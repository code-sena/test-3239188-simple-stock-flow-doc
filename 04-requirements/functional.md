# Functional Requirements — Simple Stock Flow

* **FR-01 (Product Management):** The system shall allow administrators to create, update, and logically delete products in the catalog with strict validation for unique SKUs and positive pricing.
* **FR-02 (Stock Validation):** The system shall validate that the quantity requested in a sales transaction does not exceed the current available stock before confirming checkout.
* **FR-03 (Movement Logging):** The system shall generate an immutable record in the stock movement table whenever a sale, intake, or manual adjustment is executed.
* **FR-04 (Calculation Engine):** The system shall automatically calculate the total sale amount based on unit quantities and the current active unit price, applying applicable business rules.
* **FR-05 (Role-Based Access):** The system shall restrict administrative functions (such as catalog creation and audit logs) to users with administrator privileges.
