---
name: observability-driven-development
description: Patterns for embedding observability into application code at development time, including OpenTelemetry instrumentation, SLI-aware coding practices, and structured logging conventions. Use this skill when instrumenting application code or designing SLIs at the code level.
license: MIT
---

# Observability-Driven Development

This skill provides guidance for embedding observability into application code during development, not as an operational afterthought. It covers the three telemetry signals — traces, metrics, and logs — with a focus on OpenTelemetry SDK usage and SLI-aware instrumentation patterns.

## Core Principles

1. **Instrument at Write Time, Not After**: Observability is a development concern, not an operations afterthought. Add spans, metrics, and structured log fields during initial implementation, not as a retrofit.
2. **SLI-Driven Instrumentation**: Every instrumentation point must trace back to a defined SLI. If a span or metric does not contribute to measuring an SLI, question whether it belongs.
3. **Context Propagation is Non-Negotiable**: Distributed traces are only useful when context (trace ID, span ID, baggage) propagates correctly across every service boundary, message queue, and async handler.
4. **Cardinality Discipline**: High-cardinality labels (user IDs, request IDs) on metrics cause storage explosion and query degradation. Use traces for high-cardinality investigation; keep metric labels bounded.

## Telemetry Signals

Identify the telemetry signal you are working with and consult the appropriate reference file.

### 1. Distributed Tracing
**Focus**: Instrumenting application code with spans and propagating trace context across service boundaries.
**Use when**: Adding tracing to a new service, designing span hierarchies, configuring sampling strategies, or debugging context propagation issues.
**Read**: `references/distributed-tracing-instrumentation.md`

### 2. Metrics & SLI Coding
**Focus**: Implementing application-level metrics that directly measure Service Level Indicators.
**Use when**: Adding RED or USE metrics to application code, designing histogram buckets, or implementing SLI measurement counters.
**Read**: `references/metrics-and-sli-coding.md`

### 3. Structured Logging
**Focus**: Implementing consistent, machine-parseable logging with trace correlation.
**Use when**: Establishing logging conventions, adding correlation fields, implementing log-to-trace linking, or designing log sampling strategies.
**Read**: `references/structured-logging-conventions.md`
