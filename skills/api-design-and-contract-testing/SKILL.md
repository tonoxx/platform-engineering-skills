---
name: api-design-and-contract-testing
description: OpenAPI and gRPC schema design best practices, consumer-driven contract testing with Pact, and API versioning strategies. Use this skill when designing new APIs, reviewing API schemas, or establishing contract testing between services.
license: MIT
---

# API Design & Contract Testing

This skill provides structured guidance for designing robust APIs, validating schemas in CI, and evolving APIs safely. It covers RESTful services with OpenAPI, gRPC services with Protocol Buffers, and consumer-driven contract testing practices.

## Core Principles

1. **Consumer-Driven Design**: APIs exist to serve consumers. Design schemas based on documented consumer use cases, not provider implementation details.
2. **Contracts as Executable Specifications**: API contracts (OpenAPI specs, Protobuf definitions) are not documentation artifacts — they are executable specifications that must be validated in CI on every change.
3. **Versioning is a Compatibility Strategy**: Every versioning decision (URL path, header, field deprecation) is a backward compatibility commitment. Choose the strategy that minimizes consumer disruption while allowing provider evolution.

## API Domains

Identify the API technology and activity, then consult the appropriate reference file.

### 1. REST & OpenAPI Design
**Focus**: Designing RESTful APIs with well-structured OpenAPI specifications.
**Use when**: Creating new REST endpoints, reviewing OpenAPI schemas, defining error response formats, or generating API documentation.
**Read**: `references/openapi-and-rest-design.md`

### 2. gRPC & Protobuf Design
**Focus**: Designing gRPC services with Protocol Buffer schemas.
**Use when**: Defining Protobuf messages and services, choosing between streaming and unary RPCs, or ensuring backwards-compatible schema evolution.
**Read**: `references/grpc-and-protobuf-design.md`

### 3. Contract Testing & Versioning
**Focus**: Validating API contracts between consumers and providers, and managing API lifecycle.
**Use when**: Setting up Pact contract tests, integrating schema linting into CI, choosing a versioning strategy, or planning API deprecation.
**Read**: `references/contract-testing-and-versioning.md`
