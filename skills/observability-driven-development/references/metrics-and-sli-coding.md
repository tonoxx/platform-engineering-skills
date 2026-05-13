# Metrics & SLI Coding

This document covers application-level metric implementation patterns, SLI measurement techniques, and cardinality management.

## 1. RED Metrics (Request-Oriented Services)

### Rate
- **Request Counter**: Implement a monotonic counter that increments on every inbound request. Label by endpoint, method, and response status class (2xx, 4xx, 5xx) to enable per-route analysis.
- **Throughput Monitoring**: Derive requests-per-second from the counter using rate functions in the query layer. Sudden drops in rate often indicate upstream issues or routing changes.

### Errors
- **Error Counter**: Maintain a separate counter for requests resulting in errors. Distinguish between client errors (4xx) and server errors (5xx) as they have different operational implications.
- **Error Ratio SLI**: Calculate the error ratio as (total requests - successful requests) / total requests. This directly measures availability and can be compared against SLO targets.

### Duration
- **Latency Histogram**: Record request duration as a histogram. Choose bucket boundaries that reflect meaningful latency thresholds for the service (e.g., 5ms, 10ms, 25ms, 50ms, 100ms, 250ms, 500ms, 1s, 2.5s, 5s, 10s).
- **Percentile SLIs**: Derive p50, p95, and p99 latency from the histogram. SLO targets typically reference p95 or p99 to capture tail latency behavior.

## 2. USE Metrics (Infrastructure-Oriented Services)

### Utilization
- **Resource Gauges**: Report current utilization of bounded resources (connection pool usage, thread pool active count, queue depth) as gauges. These reveal saturation before failures occur.

### Saturation
- **Queue Length**: Monitor the length of internal work queues. Growing queues indicate that the service is receiving work faster than it can process, signaling an approaching capacity limit.
- **Rejection Counters**: Count requests rejected due to capacity limits (connection pool exhausted, rate limiter triggered). Rejection indicates the service is already saturated.

### Errors
- **Internal Error Counter**: Track internal errors separate from request errors — database connection failures, timeout exceptions, circuit breaker trips. These indicate infrastructure health problems.

## 3. Cardinality Management

### Label Discipline
- **Bounded Labels**: Restrict metric labels to low-cardinality dimensions (HTTP method, status code, service name, environment). Verify that each label has a known, finite set of values.
- **Avoid Unbounded Labels**: Never use user IDs, request IDs, IP addresses, or free-text fields as metric labels. A single unbounded label can generate millions of time series.
- **Label Review**: Audit metric labels during code review. Calculate the worst-case cardinality (product of all label value counts) and reject metrics that could exceed storage capacity.

### Aggregation Strategies
- **Pre-Aggregation**: Aggregate high-cardinality data at the application level before exporting as metrics. For example, count requests by endpoint rather than by individual URL path with variable segments.
- **Exemplars**: Use OpenTelemetry exemplars to attach a trace ID to specific metric data points. This provides a path from an aggregated metric to the underlying trace without inflating metric cardinality.
