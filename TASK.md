# Task: Database Access with Exposed — Basics

Replace the manual dependency injection with Ktor's DI plugin and prepare the project for database integration with Exposed.

## Switch to Ktor DI Plugin

Update `Application.kt` to use Ktor's `dependencies { }` DSL:

1. Add a `configure()` function that sets up dependencies using:
   ```
   dependencies {
       provide<CustomerRepository> { FakeCustomerRepository() }
   }
   ```
2. Register the `configure` function in `application.yaml` as a module.

## Update Routes to Use DI

Modify `CustomerRoutes.kt` so that instead of accepting the repository as a function parameter, it resolves it from the DI container:

```kotlin
val repository: CustomerRepository by dependencies
```

This removes the need for manual wiring in `Application.kt`.

## Update Configuration

In `application.yaml`, add the `configure` module to the list of modules that Ktor loads on startup, alongside the existing `module` entry.

Keep the `FakeCustomerRepository` as the implementation for now — the real database will be added in a later step. The tests should continue to pass without modification.
