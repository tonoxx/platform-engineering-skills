# Distributed Tracing Instrumentation

This document covers OpenTelemetry SDK setup, span design patterns, context propagation, and sampling strategies at the application code level.

## 1. OpenTelemetry SDK Setup

### Instrumentation Approaches
- **Auto-Instrumentation**: Use OpenTelemetry auto-instrumentation libraries to automatically capture spans for common frameworks (HTTP servers, database clients, gRPC). This provides baseline visibility with minimal code changes.
- **Manual Spans**: Add custom spans for business-critical operations that auto-instrumentation does not cover (e.g., payment processing, cache lookups, external API calls). Manual spans provide domain-specific context.
- **Hybrid Approach**: Start with auto-instrumentation for infrastructure-level tracing, then layer manual spans for business logic. This balances coverage with development effort.

### Exporter Configuration
- **OTLP Exporter**: Configure the OpenTelemetry SDK to export spans via OTLP (gRPC or HTTP) to an OpenTelemetry Collector. Avoid exporting directly to backend-specific APIs to maintain vendor flexibility.
- **Batch Processing**: Use the BatchSpanProcessor to buffer and batch-export spans, reducing network overhead and minimizing performance impact on the application.

## 2. Span Design Patterns

### Naming Conventions
- **Operation-Based Names**: Name spans after the operation they represent (e.g., "db.query", "http.request", "payment.process"), not the function or class name. This creates meaningful trace visualizations.
- **Consistent Prefixes**: Use consistent prefixes by domain (db.*, http.*, cache.*, queue.*) to enable filtering and grouping in trace visualization tools.

### Attributes and Events
- **Semantic Conventions**: Follow OpenTelemetry semantic conventions for span attributes (http.method, db.system, rpc.service). Custom attributes should use a project-specific namespace.
- **Span Events**: Use span events to record significant moments within a span (e.g., "cache miss", "retry attempt") without creating separate child spans. Events add context without increasing span cardinality.
- **Status Codes**: Set span status to ERROR only for unhandled or unexpected failures. Expected error paths (e.g., validation rejection) should not mark the span as failed.

## 3. Context Propagation

### Cross-Service Propagation
- **W3C Trace Context**: Use W3C Trace Context (traceparent, tracestate headers) as the default propagation format for HTTP-based communication. It is the industry standard supported by all major observability vendors.
- **gRPC Metadata**: For gRPC services, context propagates via gRPC metadata headers. Ensure the OpenTelemetry gRPC interceptor is installed on both client and server.
- **Message Queue Propagation**: Inject trace context into message headers (Kafka headers, SQS message attributes, RabbitMQ headers) when producing messages. Extract context when consuming to maintain trace continuity.

## 4. Sampling Strategies

### Approaches
- **Head-Based Sampling**: Make the sampling decision at trace creation time. Simple to implement but may miss interesting traces (errors, slow requests) that were not sampled.
- **Tail-Based Sampling**: Make the sampling decision after the trace is complete, based on observed attributes (duration, error status). Requires an OpenTelemetry Collector with the tail_sampling processor.
- **Priority Sampling**: Always sample traces that match specific criteria (error status, high latency, specific user segments) regardless of the base sampling rate.
