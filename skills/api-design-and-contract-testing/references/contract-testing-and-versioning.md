# Contract Testing & Versioning

This document covers consumer-driven contract testing, schema validation in CI, and API versioning strategies.

## 1. Consumer-Driven Contract Testing

### Pact Framework
- **Consumer Tests**: The consumer defines the expected interactions (request and response pairs) as a Pact contract. These tests run without the actual provider, using a mock server generated from the contract.
- **Provider Verification**: The provider runs the Pact contracts against its actual implementation to verify that all consumer expectations are met. Failures indicate a breaking change.
- **Pact Broker**: Use a Pact Broker to share contracts between consumer and provider CI pipelines. The Broker tracks contract versions, verification results, and deployment compatibility.

### CI Integration
- **Consumer Pipeline**: Generate and publish the Pact contract on every consumer CI build. Tag the contract with the consumer's branch or environment.
- **Provider Pipeline**: On every provider CI build, fetch the latest consumer contracts from the Pact Broker and run verification. Block deployment if any contract is broken.
- **Can-I-Deploy**: Use the Pact Broker's can-i-deploy tool to verify that the specific versions of consumer and provider being deployed are mutually compatible.

## 2. Schema Validation in CI

### OpenAPI Linting
- **Spectral**: Use Spectral to lint OpenAPI specifications against configurable rulesets. Enforce naming conventions, require descriptions, validate response structures, and detect anti-patterns.
- **Custom Rules**: Define project-specific linting rules (e.g., "all endpoints must return Problem Details for errors", "pagination must use cursor-based format") to enforce organizational API standards.

### Protobuf Linting
- **Buf**: Use buf lint to enforce Protobuf style guide rules (field naming, package structure, reserved field usage). Use buf breaking to detect backwards-incompatible changes between schema versions.
- **CI Gate**: Run buf lint and buf breaking as CI checks on every pull request that modifies proto files. Block merges that introduce lint violations or breaking changes.

## 3. API Versioning Strategies

### Versioning Approaches
- **URI Path Versioning**: Include the version in the URL path (e.g., /v1/users, /v2/users). Simple and explicit but requires maintaining multiple route registrations.
- **Header Versioning**: Use a custom header (e.g., API-Version: 2) or Accept header content negotiation. Keeps URLs clean but is less discoverable.
- **Query Parameter Versioning**: Pass the version as a query parameter (e.g., ?version=2). Easy to implement but semantically imprecise since the version is not part of the resource identity.

### Deprecation Workflow
- **Sunset Header**: Add a Sunset HTTP header to responses from deprecated endpoints, indicating the date after which the endpoint will be removed (RFC 8594).
- **Deprecation Notice Period**: Communicate deprecation timelines to consumers well in advance. Monitor usage of deprecated endpoints and reach out to active consumers before removal.
- **Breaking Change Detection**: Automate breaking change detection in CI by comparing the current API specification against the last released version. Flag removals, type changes, and required field additions.
