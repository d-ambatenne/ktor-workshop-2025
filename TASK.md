# Task: Implement CRUD Routes

Create a new file `backend/src/main/kotlin/CustomerRoutes.kt` that implements CRUD routes for the `Customer` resource. Also wire the routes into the application.

## Routes to implement

Create a function `fun Routing.configureCustomerRoutes()` that sets up these endpoints under the `/customers` path:

1. **GET /customers** — Return all customers as a JSON array.
2. **POST /customers** — Accept a `CreateCustomer` JSON body, create a new `Customer` with a random `Int` ID and `Clock.System.now()` as the creation timestamp, and return it with status `201 Created`.
3. **GET /customers/{id}** — Return a specific customer by ID, or `404 Not Found` if not found.
4. **PUT /customers/{id}** — Accept an `UpdateCustomer` JSON body, update only the non-null fields on the existing customer, and return the updated customer. Return `404` if not found.
5. **DELETE /customers/{id}** — Remove the customer by ID and respond with `204 No Content`. Return `404` if not found.

## Storage

Use an in-memory `MutableList<Customer>` at the top level of the file to store customers. This is temporary — it will be replaced with a database later.

## Wiring

In `Application.kt`, call `configureCustomerRoutes()` inside the existing `routing { }` block.

Use `call.respond()`, `call.receive()`, and `call.parameters["id"]` for request handling. Use `kotlinx.datetime.Clock` for timestamps and `kotlin.random.Random` for ID generation. Look at the existing route definitions in `Application.kt` for the project's patterns.
