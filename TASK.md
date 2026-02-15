# Task: Write First Endpoint Tests

Implement the CRUD test methods in `backend/src/test/kotlin/ApplicationTest.kt`. The test class already has a shared `TestApplication` and `HttpClient` set up in a companion object, along with four empty test stubs and a list of fake `Customer` data.

Fill in the test implementations:

1. **`get all data()`** — Send a GET request to `/customers`. Assert that the response status is `200 OK` and that the response body deserializes to the expected list of customers.

2. **`post data instance()`** — Send a POST request to `/customers` with a `CreateCustomer` JSON body. Assert that the response status is `201 Created` and that the returned `Customer` has the correct name and email.

3. **`put data instance()`** — Send a PUT request to `/customers/{id}` with an `UpdateCustomer` JSON body to update an existing customer's fields. Assert that the response reflects the updated values.

4. **`delete data instance()`** — Send a DELETE request to `/customers/{id}`. Assert that the delete succeeds, and that a subsequent GET for that customer returns `404 Not Found`.

Use `assertEquals` for assertions. The `HttpClient` is already configured with `ContentNegotiation` and `kotlinx.serialization.json`, so you can use `setBody()` for request bodies and `.body<T>()` for response deserialization. Look at the existing companion object for the fake test data (customers: Anton, Leonid, Simon).
