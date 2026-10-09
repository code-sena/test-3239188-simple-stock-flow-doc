# Domain Glossary (Ubiquitous Language) — Simple Stock Flow

* **Product (Producto):** An item available in the inventory catalog, possessing a unique identifier, name, category, unit price, and current stock count.
* **Category (Categoría):** A logical classification used to group related products in the catalog.
* **Sale (Venta):** A recorded commercial transaction representing the transfer of goods, capturing the timestamp, total price, and operator details.
* **Sale Item (Ítem de Venta):** An individual line entry belonging to a sale, specifying the product, purchased quantity, and frozen unit price.
* **User (Usuario):** An internal operator authorized to interact with the system under a defined security role (`admin` or `seller`).
* **Stock Invariant (Invariante de Stock):** The strict business rule ensuring inventory levels never drop below zero (`stock >= 0`).
