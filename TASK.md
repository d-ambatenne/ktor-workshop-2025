# Task: Add Exposed DAO Entities — Booking Relations

Extend the application with a `Booking` entity that has a many-to-one relationship with `Customer`.

## New Domain Classes

Add to `Domain.kt`:

- `Booking` data class: `id: Int`, `customerId: Int`, `bookingDate: Instant`, `amount: Double`. Make it `@Serializable`.
- `CreateBooking` data class: `customerId: Int`, `amount: Double`. Make it `@Serializable`.
- `CustomerWithBooking` data class: all `Customer` fields plus `bookings: List<Booking>`. Make it `@Serializable`.

## Bookings Table

In `CustomerRepositoryImpl.kt`, define a new Exposed table:

```kotlin
object Bookings : IntIdTable("bookings") {
    val customerId = integer("customer_id").references(Customers.id)
    val bookingDate = timestamp("booking_date").defaultExpression(CurrentTimestamp)
    val amount = double("amount")
}
```

## Updated Repository Interface

Add to `CustomerRepository`:
- `createBooking(id: Int, amount: Double): Booking`

Change the return types:
- `findAll()` should return `List<CustomerWithBooking>` instead of `List<Customer>`
- `find(id)` should return `CustomerWithBooking?` instead of `Customer?`

## Repository Implementation

In `CustomerRepositoryImpl`:
- Implement `createBooking()` — insert into Bookings table and return the new `Booking`
- Update `findAll()` and `find()` to also fetch each customer's bookings using a query on the `Bookings` table filtered by `customerId`
- Create helper mapping functions: `ResultRow.toBooking()` and a function to build `CustomerWithBooking` from a customer row plus its bookings

## Database Migration

Create `backend/src/main/resources/db/migration/V2__create_booking_table.sql`:

```sql
CREATE TABLE bookings (
    booking_id SERIAL PRIMARY KEY,
    customer_id INT NOT NULL,
    booking_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    amount DOUBLE NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

Update the tests to work with the new `CustomerWithBooking` return types.