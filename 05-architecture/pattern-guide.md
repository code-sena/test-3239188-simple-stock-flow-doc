# Design Patterns Guide — Simple Stock Flow

The following patterns are mandatory for the C# and .NET implementation of the system:

1. **Repository Pattern:** Abstraction of data access through interfaces in the application/domain layer, decoupling business logic from Entity Framework Core.
2. **Value Objects:** Used for immutable domain concepts such as `Money` (strictly positive unit price) and `Quantity` (quantities in sale lines).
3. **Specification / Guard Clauses:** Early validation of business invariants before modifying entity states like `Product` or `Sale`.
4. **Dependency Injection (DI):** Native .NET container to inject application services, repositories, and database contexts into API controllers.
