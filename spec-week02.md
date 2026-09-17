# Books API Week 02 Spec - Version 1

## Feature 1: Book CRUD Operations and Author References

### Goal

Update the existing Week 01 book API so book documents include a reference to an author and the API supports all CRUD operations for books. Every book route must be documented and testable in Swagger.

### Data Model

Book documents will be stored in the `books` collection.

Required book fields:

- `id`: string, required, custom id such as `b1`
- `authorId`: string, required, references the `id` field of an author document
- `title`: string, required
- `publicationDate`: string, required

Books will continue to use custom string ids instead of MongoDB `_id` values for route parameters.

### Relationship to Authors

Each book will identify its author with an `authorId` field. The value of `authorId` must match the custom `id` value of an existing author document.

When creating or updating a book, the API should reject the request with a `400` status code if the submitted `authorId` does not match an existing author.

### Routes

#### GET /books

Purpose: Return all books.

Success:

- Status code: `200`
- Response body: an array of book objects

Errors:

- `500` if an unexpected server or database error occurs

#### GET /books/:id

Purpose: Return one book by its custom id.

Success:

- Status code: `200`
- Response body: the matching book object

Errors:

- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

#### POST /books

Purpose: Create a new book.

Request body:

    {
      "id": "b4",
      "authorId": "a1",
      "title": "Example Book Title",
      "publicationDate": "2026-01-15"
    }

Success:

- Status code: `201`
- Response body: the newly created book object

Errors:

- `400` if a required field is missing
- `400` if the `id` already exists
- `400` if the `authorId` does not match an existing author
- `500` if an unexpected server or database error occurs

#### PUT /books/:id

Purpose: Update an existing book.

Request body:

    {
      "authorId": "a2",
      "title": "Updated Book Title",
      "publicationDate": "2026-02-20"
    }

Success:

- Status code: `200`
- Response body: the updated book object

Errors:

- `400` if a required field is missing
- `400` if the `authorId` does not match an existing author
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

#### DELETE /books/:id

Purpose: Delete an existing book.

Success:

- Status code: `204`
- Response body: none

Errors:

- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

### Swagger Documentation

Swagger must document every book route.

### Deployment Expectations

After implementation, the book routes must work locally and from the deployed Render application. The deployed Swagger page at `/api-docs` must allow someone to test every book route from the browser.

Books API Week 02 Spec - Version 2
==================================

Feature 1: Book CRUD Operations and Author References
-----------------------------------------------------

### Goal

Update the existing Week 01 book API so book documents include a reference to an author and the API supports all CRUD operations for books. Every book route must be documented and testable in Swagger.

### Data Model

Book documents will be stored in the `books` collection.

Required book fields:

-   `id`: string, required, custom id such as `b1`

-   `authorId`: string, required, references the `id` field of an author document

-   `title`: string, required

-   `publicationDate`: string, required

Books will continue to use custom string ids instead of MongoDB `_id` values for route parameters.

The `id` value must be unique within the `books` collection.

The `title`, `authorId`, and `publicationDate` fields must contain non-empty values of the correct type.

The `publicationDate` field will be stored as a string using the `YYYY-MM-DD` format.

### Relationship to Authors

Each book will identify its author with an `authorId` field. The value of `authorId` must match the custom `id` value of an existing author document.

When creating or updating a book, the API must verify that the submitted `authorId` matches an existing author.

If the submitted `authorId` does not match an existing author, the API will reject the request with a `400` status code.

### Routes

#### GET /books

Purpose: Return all books.

Success:

-   Status code: `200`

-   Response body: an array of book objects

Errors:

-   `500` if an unexpected server or database error occurs

#### GET /books/:id

Purpose: Return one book by its custom id.

The `id` in the URL must match the custom `id` field of the requested book.

Success:

-   Status code: `200`

-   Response body: the matching book object

Errors:

-   `404` if no book exists with that id

-   `500` if an unexpected server or database error occurs

#### POST /books

Purpose: Create a new book.

Request body:

```
{
  "id": "b4",
  "authorId": "a1",
  "title": "Example Book Title",
  "publicationDate": "2026-01-15"
}

```

The request body must contain all four required fields.

Success:

-   Status code: `201`

-   Response body: the newly created book object

Errors:

-   `400` if a required field is missing

-   `400` if a required field has an invalid type or empty value

-   `400` if the `id` already exists

-   `400` if the `authorId` does not match an existing author

-   `400` if `publicationDate` is not in the required `YYYY-MM-DD` format

-   `500` if an unexpected server or database error occurs

#### PUT /books/:id

Purpose: Update an existing book.

The `id` in the URL identifies the book being updated. The custom book id cannot be changed by the request.

Request body:

```
{
  "authorId": "a2",
  "title": "Updated Book Title",
  "publicationDate": "2026-02-20"
}

```

The request body must contain all three editable fields.

Success:

-   Status code: `200`

-   Response body: the updated book object

Errors:

-   `400` if a required field is missing

-   `400` if a required field has an invalid type or empty value

-   `400` if the `authorId` does not match an existing author

-   `400` if `publicationDate` is not in the required `YYYY-MM-DD` format

-   `404` if no book exists with that id

-   `500` if an unexpected server or database error occurs

#### DELETE /books/:id

Purpose: Delete an existing book.

The `id` in the URL identifies the book to delete.

Success:

-   Status code: `204`

-   Response body: none

Errors:

-   `404` if no book exists with that id

-   `500` if an unexpected server or database error occurs

### Request Validation

Book create and update requests must be JSON objects.

If the request body is missing, malformed, or does not contain the required fields, the API must return an appropriate `400` status code.

The API must validate all required fields for the correct type and reject empty required values.

The API must not allow the `id` to be changed through a PUT request.

### Swagger Documentation

Swagger must document every book route.

Swagger documentation must include:

-   The HTTP method and route path

-   A summary of the route's purpose

-   The `id` path parameter for routes that use it

-   Required request body fields for POST and PUT

-   Example request bodies for POST and PUT

-   Successful response status codes

-   Error response status codes

-   Descriptions of the expected response bodies

Swagger documentation must accurately match the behavior implemented by the controllers.

### Deployment Expectations

After implementation, the book routes must work locally and from the deployed Render application.

The deployed Swagger page at `/api-docs` must allow someone to test every book route from the browser.

The generated `swagger.json` file must be regenerated and committed whenever the API routes or OpenAPI documentation comments are changed.

Feature 2: Author CRUD Operations
---------------------------------

### Goal

Add an authors collection to the Books API. The API will support all CRUD operations for authors. Authors will use custom string ids, and books will reference authors through the `authorId` field. Every author route must be documented and testable in Swagger.

### Data Model

Author documents will be stored in the `authors` collection.

Required author fields:

-   `id`: string, required, custom id such as `a1`

-   `name`: string, required

-   `birthYear`: number, required

Authors will continue to use custom string ids instead of MongoDB `_id` values for route parameters.

The `id` value must be unique within the `authors` collection.

The `name` field must contain a non-empty string.

The `birthYear` field must contain a valid number representing a four-digit year.

### Relationship to Books

Books reference authors using the book's `authorId` field.

The value of a book's `authorId` must match the custom `id` value of an existing author document.

When an author is deleted, the API must first check whether any books reference that author's id.

If one or more books reference the author, the API must not delete the author and must return a `409` status code.

### Routes

#### GET /authors

Purpose: Return all authors.

Success:

-   Status code: `200`

-   Response body: an array of author objects

Errors:

-   `500` if an unexpected server or database error occurs

#### GET /authors/:id

Purpose: Return one author by its custom id.

The `id` in the URL must match the custom `id` field of the requested author.

Success:

-   Status code: `200`

-   Response body: the matching author object

Errors:

-   `404` if no author exists with that id

-   `500` if an unexpected server or database error occurs

#### POST /authors

Purpose: Create a new author.

Request body:

```
{
  "id": "a2",
  "name": "George Orwell",
  "birthYear": 1903
}

```

The request body must contain all three required fields.

Success:

-   Status code: `201`

-   Response body: the newly created author object

Errors:

-   `400` if a required field is missing

-   `400` if a required field has an invalid type or empty value

-   `400` if the `id` already exists

-   `400` if `birthYear` is not a valid four-digit year

-   `500` if an unexpected server or database error occurs

#### PUT /authors/:id

Purpose: Update an existing author.

The `id` in the URL identifies the author being updated. The custom author id cannot be changed by the request.

Request body:

```
{
  "name": "George Orwell",
  "birthYear": 1903
}

```

The request body must contain both editable fields.

Success:

-   Status code: `200`

-   Response body: the updated author object

Errors:

-   `400` if a required field is missing

-   `400` if a required field has an invalid type or empty value

-   `400` if `birthYear` is not a valid four-digit year

-   `404` if no author exists with that id

-   `500` if an unexpected server or database error occurs

#### DELETE /authors/:id

Purpose: Delete an existing author.

The `id` in the URL identifies the author to delete.

Before deleting the author, the API must check whether any books reference the author's id.

If one or more books reference the author, the API must not delete the author.

Success:

-   Status code: `204`

-   Response body: none

Errors:

-   `404` if no author exists with that id

-   `409` if one or more books still reference the author

-   `500` if an unexpected server or database error occurs

### Request Validation

Author create and update requests must be JSON objects.

If the request body is missing, malformed, or does not contain the required fields, the API must return an appropriate `400` status code.

The API must validate all required fields for the correct type and reject empty required values.

The API must not allow the `id` to be changed through a PUT request.

The `birthYear` value must be a number representing a four-digit year.

### Swagger Documentation

Swagger must document every author route.

Swagger documentation must include:

-   The HTTP method and route path

-   A summary of the route's purpose

-   The `id` path parameter for routes that use it

-   Required request body fields for POST and PUT

-   Example request bodies for POST and PUT

-   Successful response status codes

-   Error response status codes

-   Descriptions of the expected response bodies

Swagger documentation must accurately match the behavior implemented by the controllers.

### Deployment Expectations

After implementation, the author routes must work locally and from the deployed Render application.

The deployed Swagger page at `/api-docs` must allow someone to test every author route from the browser.

The generated `swagger.json` file must be regenerated and committed whenever the API routes or OpenAPI documentation comments are changed.
