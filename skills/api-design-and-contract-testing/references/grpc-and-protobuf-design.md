# gRPC & Protobuf Design

This document covers Protocol Buffer schema design, gRPC service patterns, and backwards-compatible schema evolution.

## 1. Protocol Buffer Style Guide

### Field Management
- **Field Numbering**: Assign field numbers sequentially. Numbers 1-15 use one byte on the wire — reserve these for frequently used fields to minimize message size.
- **Reserved Fields**: When removing a field, mark its number and name as reserved to prevent future reuse. Reusing a field number with a different type causes silent data corruption.
- **Oneof Usage**: Use oneof for mutually exclusive fields to enforce that only one variant is set. This clarifies the message semantics and reduces payload size.

### Message Design
- **Wrapper Messages**: Wrap request and response types in dedicated messages rather than using primitive types directly. This allows adding fields later without breaking the service signature.
- **Enum Best Practices**: Always include an UNSPECIFIED value at position 0 for every enum. This serves as the default and distinguishes between "not set" and the first meaningful value.
- **Nested vs Top-Level**: Define messages at the top level when they are reused across multiple services. Use nested messages only for types tightly coupled to a single parent message.

## 2. gRPC Service Patterns

### RPC Selection Criteria
- **Unary RPC**: Use for simple request-response interactions where the client sends one message and receives one response. Suitable for CRUD operations and queries.
- **Server Streaming**: Use when the server needs to push a sequence of messages to the client (e.g., real-time updates, large result sets). The client sends one request and reads a stream of responses.
- **Client Streaming**: Use when the client needs to send a stream of data to the server (e.g., file uploads, batch inserts). The server processes the stream and returns a single response.
- **Bidirectional Streaming**: Use for real-time conversational protocols where both client and server send messages independently (e.g., chat, collaborative editing).

### gRPC-Gateway for REST Interop
- **HTTP/JSON Facade**: Use gRPC-Gateway to generate a reverse proxy that translates RESTful HTTP/JSON requests into gRPC calls. This enables a single gRPC service to serve both gRPC and REST clients.
- **Annotation Conventions**: Define HTTP method and path mappings using google.api.http annotations in the proto file. Map resource-oriented URL patterns to the corresponding RPC methods.

## 3. Backwards-Compatible Schema Evolution

### Safe Changes
- **Adding Fields**: Adding a new optional field with a new field number is always safe. Existing clients ignore unknown fields.
- **Adding Enum Values**: Adding new values to an existing enum is safe if clients handle unknown enum values gracefully (most Protobuf libraries default to 0).
- **Adding RPCs**: Adding new RPC methods to a service is safe. Existing clients are unaffected.

### Breaking Changes
- **Changing Field Types**: Changing the type of an existing field (e.g., int32 to string) breaks wire compatibility. Use a new field number instead.
- **Renaming Fields**: Renaming a field in proto3 does not break wire compatibility (numbers are used on the wire), but breaks JSON serialization where field names are used as keys.
- **Removing RPCs**: Removing an RPC method breaks all clients that call it. Deprecate the method first and monitor usage before removal.
