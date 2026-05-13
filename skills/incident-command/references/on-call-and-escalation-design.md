# On-Call & Escalation Design

This document covers on-call rotation patterns, escalation tier definitions, severity classification, and alerting integration.

## 1. On-Call Rotation Patterns

### Rotation Structures
- **Primary/Secondary**: Assign a primary on-call responder with a secondary backup. The secondary takes over if the primary does not acknowledge an alert within the defined time window.
- **Follow-the-Sun**: Distribute on-call across geographic regions so that responders are always working during their local business hours. Requires at least three time-zone-distributed teams.
- **Rotation Duration**: Weekly rotations are the most common. Shorter rotations (3-4 days) reduce fatigue but increase handoff frequency. Adjust based on alert volume and team size.

### Sustainability
- **Compensation**: Provide on-call compensation (additional pay, time off in lieu, or reduced workload during on-call weeks) to acknowledge the burden and prevent burnout.
- **Alert Volume Management**: Track the number of pages per on-call shift. If the average exceeds two actionable alerts per night, prioritize alert tuning and automation to reduce volume.
- **Handoff Procedures**: Require a structured handoff at each rotation change that includes active incidents, ongoing investigations, and any known risks or upcoming changes.

## 2. Escalation Tier Definitions

### Tier Structure
- **L1 Triage**: First responder acknowledges the alert, assesses severity, and attempts resolution using documented runbooks. If unresolved within the defined time window, escalate to L2.
- **L2 Domain Expert**: Engineers with deep knowledge of the affected system join the response. They perform root cause analysis and implement fixes beyond runbook coverage.
- **L3 Engineering Leadership**: Engaged for SEV1 incidents or when L2 cannot resolve within the escalation window. L3 has authority to make architectural decisions, approve emergency changes, and coordinate cross-team resources.

### Time-Based Escalation
- **Auto-Escalation**: Configure alerting tools to automatically escalate to the next tier if an alert is not acknowledged within the defined SLA (e.g., 5 minutes for SEV1, 15 minutes for SEV2).
- **Management Notification**: Automatically notify engineering management for all SEV1 incidents and for any incident exceeding its expected resolution time by more than 50%.

## 3. Severity Classification

### Classification Criteria
- **SEV1 (Critical)**: Complete service outage affecting all users, data loss or corruption, or security breach. Requires immediate all-hands response and executive notification.
- **SEV2 (Major)**: Significant degradation affecting a large subset of users. Core functionality is impaired but workarounds may exist. Requires immediate response during business hours and within one hour outside business hours.
- **SEV3 (Minor)**: Partial degradation affecting a small subset of users or non-critical functionality. Responded to during business hours with a target resolution within one business day.
- **SEV4 (Low)**: Cosmetic issues, minor bugs, or informational alerts that do not impact users. Addressed as part of normal sprint work.

## 4. Alerting Integration

### Tool Configuration
- **PagerDuty/Opsgenie**: Configure escalation policies that mirror the tier structure. Set up on-call schedules, override capabilities, and integration with monitoring tools (Prometheus Alertmanager, CloudWatch, Datadog).
- **Alert Routing**: Route alerts to the appropriate team based on service ownership, not technology stack. Use service catalogs (Backstage, PagerDuty Service Directory) to maintain accurate ownership mappings.
