# Task: Database Access with Exposed — Adding Relations

Replace the fake in-memory repository with a real database implementation using Exposed R2DBC and H2.

## Database Setup

Create a new file `backend/src/main/kotlin/org/jetbrains/plugins/Database.kt` with a `setupDatabase(config: ApplicationConfig)` function that:

1. Creates an H2 in-file database connection using the configuration from `application.yaml` (username, JDBC URL).
2. Configures Flyway for database migrations with `baselineOnMigrate = true` and migration files from `classpath:db/migration`.
3. Connects Exposed R2DBC to the database using `Database.connect()` with the R2DBC URL.
4. Subscribes to `ApplicationStopped` to close the database connection on shutdown.

## Customers Table

In `CustomerRepositoryImpl.kt`, define an Exposed table object:

```kotlin
object Customers : IntIdTable("customers") {
    val name = varchar("name", 255)
    val email = varchar("email", 255).uniqueIndex()
    val createdAt = timestamp("created_at").defaultExpression(CurrentTimestamp)
}
```

## Repository Implementation

Implement `CustomerRepositoryImpl` using Exposed R2DBC's `suspendTransaction` for all database operations:

- `findAll()` — Select all rows from Customers, map to `Customer` objects
- `find(id)` — Select by ID, return null if not found
- `create(customer)` — Insert and return the created `Customer` with generated ID
- `update(id, customer)` — Update non-null fields, return updated customer
- `delete(id)` — Delete by ID, return true if a row was affected

## Database Migration

Create `backend/src/main/resources/db/migration/V1__create_customer_table.sql`:

```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Wiring

Update `Application.kt` to:
1. Call `setupDatabase(property("config.database"))` in the `configure()` function.
2. Change the DI provider from `FakeCustomerRepository` to `CustomerRepositoryImpl`.

Add the database configuration section to `application.yaml`.