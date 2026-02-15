# Task: Set Up Test Infrastructure

Set up the test infrastructure for the Ktor backend application. You need to:

1. **Define data model classes** in `backend/src/main/kotlin/Application.kt`:
   - `Customer` data class with fields: `id: Int`, `name: String`, `email: String`, `createdAt: Instant` (from `kotlinx.datetime`). Make it `@Serializable`.
   - `CreateCustomer` data class with fields: `name: String`, `email: String`. Make it `@Serializable`.
   - `UpdateCustomer` data class with fields: `name: String?`, `email: String?` (both optional). Make it `@Serializable`.

2. **Create test stubs** in `backend/src/test/kotlin/ApplicationTest.kt`:
   - Set up a shared `TestApplication` and `HttpClient` (with JSON content negotiation) in a companion object, using `runBlocking` instead of individual `testApplication` blocks.
   - Add four empty test methods:
     - `get all data()` — for testing GET /customers
     - `post data instance()` — for testing POST /customers
     - `put data instance()` — for testing PUT /customers/{id}
     - `delete data instance()` — for testing DELETE /customers/{id}

Use `kotlinx-datetime` for the `Instant` type and `kotlinx-serialization` for the `@Serializable` annotations. Look at the existing `Application.kt` and `ApplicationTest.kt` for the project's patterns and imports.
