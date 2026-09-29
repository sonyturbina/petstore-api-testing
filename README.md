# Petstore API Testing

A pet project for practicing REST API testing using Postman and Swagger Petstore.

The project demonstrates manual API testing, positive and negative test scenarios, environment variables, automated assertions, and basic defect documentation.

## API Under Test

Swagger Petstore API

Base URL:
`https://petstore.swagger.io/v2`

## Tools

- Postman
- Swagger
- JSON
- JavaScript (Postman scripts)
- GitHub

## Tested HTTP Methods

- GET — retrieve pet data
- POST — create a new pet
- PUT — update an existing pet
- DELETE — delete a pet

## Project Structure

The Postman collection is organized into three sections:

### Positive Tests

- `Create pet - POST`
- `Get pet by ID - 200`
- `Update pet - PUT`
- `Delete pet - DELETE`

### Negative Tests

- `Get pet by ID - 404 Pet not found`
- `Get pet by ID - Invalid ID`
- `Create pet - Invalid JSON`
- `Delete pet - 404 Not found`

### Bug Reports

- `BUG-001 - Invalid petId returns 404 instead of 400`

## Test Scenarios

The project covers:

- Creating a pet
- Getting a pet by ID
- Updating pet data
- Deleting a pet
- Requesting a non-existing pet
- Sending an invalid pet ID
- Sending malformed JSON
- Deleting an already deleted pet
- Boundary testing for pet IDs
- HTTP status code validation
- Response body validation

## Automated Checks

Postman scripts are used to validate API responses.

Examples:

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

Response data is also validated:

```javascript
pm.test("Pet ID is correct", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData.id).to.eql(
        Number(pm.environment.get("petId"))
    );
});
```

## Dynamic Test Data

Before creating a pet, a random 9-digit ID is generated:

```javascript
const randomPetId =
    Math.floor(Math.random() * 900000000) + 100000000;

pm.environment.set("petId", randomPetId);
```

After the POST request, the ID returned by the API is stored in the environment:

```javascript
const jsonData = pm.response.json();

pm.environment.set("petId", jsonData.id);
```

The same `petId` is then reused in GET, PUT, and DELETE requests.

This creates the following API test flow:

`POST → GET → PUT → DELETE`

## Environment Variables

The collection uses:

| Variable | Description |
|---|---|
| `base_url` | Swagger Petstore base URL |
| `petId` | ID of the pet used during testing |

After importing the environment into Postman, set:

`base_url = https://petstore.swagger.io/v2`

`petId` is generated automatically by the collection.

## Defect Found

### BUG-001 — Invalid petId returns 404 instead of 400

**Endpoint**

`GET /pet/{petId}`

**Steps to reproduce**

1. Send `GET /pet/abc`.
2. Check the HTTP status code and response body.

**Expected result**

HTTP `400 Bad Request` — Invalid ID supplied.

**Actual result**

HTTP `404 Not Found`.

The response body contains a `NumberFormatException` for the value `"abc"`.

**Severity:** Minor

The behavior differs from the response documented by the Swagger specification.

## How to Run

1. Clone or download this repository.
2. Open Postman.
3. Import `Petstore-API-Testing.postman_collection.json`.
4. Import `Petstore-Environment.postman_environment.json`.
5. Select `Petstore Environment`.
6. Set `base_url` to `https://petstore.swagger.io/v2`.
7. Run the requests from the collection.

For the positive CRUD flow, run the requests in this order:

`Create pet - POST → Get pet by ID - 200 → Update pet - PUT → Delete pet - DELETE`

## What I Practiced

During this project I practiced:

- REST API testing
- HTTP methods and status codes
- JSON request and response validation
- Positive and negative testing
- Boundary and validation testing
- Postman collections
- Postman environments and variables
- Pre-request scripts
- After-response assertions
- Dynamic test data
- CRUD test flows
- Bug reporting
- Git and GitHub project documentation
