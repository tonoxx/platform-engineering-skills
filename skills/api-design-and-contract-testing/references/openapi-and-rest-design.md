# REST & OpenAPI Design

This document covers RESTful API design conventions, OpenAPI specification best practices, and error response standardization.

## 1. OpenAPI Specification Structure

### Schema Organization
- **OpenAPI 3.1 Alignment**: Use OpenAPI 3.1 which aligns with JSON Schema 2020-12, enabling full JSON Schema compatibility including if/then/else and prefixItems.
- **Component Reuse**: Define reusable schemas, parameters, and responses in the components section. Reference them via $ref throughout the specification to ensure consistency and reduce duplication.
- **Specification Splitting**: For large APIs, split the specification into multiple files organized by resource domain. Use $ref to compose them into a single specification during build.

### Documentation Quality
- **Description Fields**: Every endpoint, parameter, and schema property should have a description field that explains the business meaning, not just the data type.
- **Examples**: Include realistic example values for request and response bodies. Examples serve as both documentation and test fixtures.

## 2. RESTful Resource Design

### Naming Conventions
- **Resource Nouns**: Use plural nouns for collection endpoints (e.g., /users, /orders). Avoid verbs in paths — the HTTP method conveys the action.
- **Nested Resources**: Limit nesting to one level (e.g., /users/{id}/orders). Deeper nesting indicates the resource should be promoted to a top-level endpoint with filtering.
- **Consistent Casing**: Use kebab-case for URL paths and camelCase for JSON property names. Apply the convention consistently across all endpoints.

### Pagination and Filtering
- **Cursor-Based Pagination**: Prefer cursor-based pagination over offset-based for large datasets. Cursors are stable across concurrent modifications and perform better at scale.
- **Filtering Parameters**: Use query parameters for filtering (e.g., ?status=active&created_after=2024-01-01). Document the available filters and their formats in the OpenAPI specification.

## 3. Error Response Standardization

### RFC 7807 Problem Details
- **Standard Format**: Adopt RFC 7807 Problem Details for HTTP APIs as the error response format. It provides a consistent structure with type, title, status, detail, and instance fields.
- **Error Type URIs**: Define stable, documented URIs for each error type. These URIs serve as machine-readable error identifiers that clients can use for programmatic error handling.
- **Validation Errors**: For request validation failures, extend the Problem Details format with an errors array containing field-level error details (field path, constraint violated, message).

### HTTP Status Code Discipline
- **Precise Status Codes**: Use specific status codes (409 Conflict, 422 Unprocessable Entity) rather than generic ones (400 Bad Request) to give clients actionable information about the failure.
- **Idempotency**: Ensure that repeated identical requests produce the same result and status code. Use idempotency keys for non-idempotent operations (POST) to prevent duplicate processing.
