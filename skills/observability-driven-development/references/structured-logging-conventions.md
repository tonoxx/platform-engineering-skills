# Structured Logging Conventions

This document covers structured logging standards, correlation field design, sensitive data handling, and log-to-trace integration.

## 1. Structured Logging Standards

### JSON Format
- **Machine-Parseable Output**: Emit all application logs as JSON objects. JSON format enables automated parsing, filtering, and aggregation by log management systems without custom parsing rules.
- **Consistent Field Naming**: Define a project-wide schema for standard fields (timestamp, level, message, service, environment). Use consistent naming conventions (camelCase or snake_case) across all services.

### Log Level Semantics
- **ERROR**: Unhandled failures requiring human investigation. The service cannot fulfill the request and no automatic recovery is possible.
- **WARN**: Unexpected conditions that the service recovers from automatically but that may indicate a developing problem (e.g., retry succeeded after timeout, fallback value used).
- **INFO**: Significant business events and state transitions (request received, order processed, deployment completed). These form the narrative of normal service operation.
- **DEBUG**: Detailed diagnostic information useful during development or active troubleshooting. Disabled in production by default to avoid log volume explosion.

## 2. Correlation Fields

### Trace Correlation
- **trace_id and span_id**: Include the current OpenTelemetry trace_id and span_id in every log entry. This enables direct navigation from a log line to the corresponding trace in the observability backend.
- **request_id**: For services that receive a client-generated request ID, propagate and log it alongside the trace context. This enables correlation with client-side logs and support tickets.

### Business Context
- **Domain-Specific Fields**: Add structured fields for business-relevant context (user_id, order_id, tenant_id) to enable filtering by business entity. Use consistent field names across services for the same entity.
- **Operation Context**: Include fields that identify the current operation (endpoint, action, workflow_step) to enable log filtering by business process rather than by technical component.

## 3. Sensitive Data Handling

### Redaction Patterns
- **Field-Level Redaction**: Identify fields that may contain PII or credentials (email, phone, auth tokens) and redact them before logging. Use allowlist-based approaches rather than blocklist to ensure new sensitive fields are caught.
- **Partial Masking**: For fields needed for debugging but containing sensitive data, apply partial masking (e.g., last 4 digits of a card number, first character of an email address) to balance debuggability with privacy.

## 4. Log-to-Trace Integration

### OpenTelemetry Logs Bridge
- **Logs Bridge API**: Use the OpenTelemetry Logs Bridge API to route application logs through the OpenTelemetry pipeline. This automatically attaches trace context and enables unified telemetry processing.
- **Existing Logger Integration**: For applications using established logging libraries (Log4j, Serilog, structlog), use the corresponding OpenTelemetry appender or handler to bridge logs without replacing the logging framework.

### Log Sampling
- **Volume-Based Sampling**: For high-throughput services, implement log sampling to reduce volume while maintaining statistical representativeness. Always exempt ERROR-level logs from sampling.
- **Dynamic Log Levels**: Implement dynamic log level adjustment (via feature flag or runtime configuration) to enable DEBUG logging for specific requests or users without redeploying the application.
