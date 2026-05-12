# Service Level Objective (SLO) Definition

## Service Overview
**Service Name**: [Name of the service]
**Owner**: [Team responsible for the service]
**Description**: [Brief description of what the service does and its business value]

## User Journey
[Describe the critical path a user takes that relies on this service. Why is this journey important?]

## Service Level Indicators (SLIs)

### SLI 1: [e.g., Availability / Success Rate]
- **Description**: [What exactly is being measured?]
- **Metric Source**: [e.g., Prometheus query, CloudWatch metric]
- **Measurement Method**: [e.g., (Successful HTTP Requests / Total HTTP Requests) * 100]
- **Where it is measured**: [e.g., Load Balancer, Application Log, Client-side]

### SLI 2: [e.g., Latency]
- **Description**: [What exactly is being measured?]
- **Metric Source**: [e.g., Prometheus query, CloudWatch metric]
- **Measurement Method**: [e.g., 95th percentile of HTTP request duration]
- **Where it is measured**: [e.g., Load Balancer, Application Log, Client-side]

## Service Level Objectives (SLOs)

| SLI | Target | Time Window |
|---|---|---|
| [SLI 1 Name] | [e.g., 99.9%] | [e.g., Rolling 30 days] |
| [SLI 2 Name] | [e.g., < 200ms for 95% of requests] | [e.g., Rolling 30 days] |

## Error Budget
- **Total Error Budget**: [e.g., 0.1% or 43.2 minutes of downtime per 30 days]
- **Current Status**: [Link to dashboard tracking the error budget]

## Error Budget Policy
[What actions are taken when the error budget is depleted? e.g., Feature freezes, prioritizing reliability work, executive review.]
