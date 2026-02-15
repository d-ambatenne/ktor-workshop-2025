# Task: Add Project Structure and Dependency Injection

Refactor the application to use a proper package structure and a repository pattern with dependency injection.

## Package reorganization

Move the application code into the `org.jetbrains` package hierarchy:

- `backend/src/main/kotlin/org/jetbrains/Application.kt` — Main application module
- `backend/src/main/kotlin/org/jetbrains/customers/Domain.kt` — Data classes (`Customer`, `CreateCustomer`, `UpdateCustomer`)
- `backend/src/main/kotlin/org/jetbrains/customers/CustomerRoutes.kt` — Route definitions
- `backend/src/main/kotlin/org/jetbrains/customers/CustomerRepository.kt` — Repository interface
- `backend/src/main/kotlin/org/jetbrains/customers/fake/FakeCustomerRepository.kt` — In-memory implementation

## Repository interface

Create a `CustomerRepository` interface with these suspend functions:

- `findAll(): List<Customer>`
- `find(id: Int): Customer?`
- `create(customer: CreateCustomer): Customer`
- `update(id: Int, customer: UpdateCustomer): Customer?`
- `delete(id: Int): Boolean`

## Fake implementation

Create `FakeCustomerRepository` that implements `CustomerRepository` using an in-memory `MutableList<Customer>`. Move the existing in-memory storage logic from the routes into this class.

## Dependency injection

In `Application.kt`, create the `FakeCustomerRepository` instance and pass it to the route configuration function: `configureCustomerRoutes(repository)`. Update `CustomerRoutes.kt` to accept a `CustomerRepository` parameter instead of using a global variable.

## Tests

Update `backend/src/test/kotlin/ApplicationTest.kt` to reflect the new package imports. The test behavior should remain the same.
