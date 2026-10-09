# Security Rules — Simple Stock Flow

* **Input Validation:** All incoming request payloads must be strictly validated at the application boundaries to prevent injection attacks.
* **Data Integrity:** Database transactions involving stock modifications must enforce ACID compliance to prevent race conditions and illegal negative balances.
