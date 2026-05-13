# Postmortem & Learning

This document covers structured postmortem facilitation, action item quality standards, and organizational learning practices.

## 1. Postmortem Facilitation

### Scheduling and Preparation
- **Timing**: Hold the postmortem within 3-5 business days of incident resolution. Too soon and details are fuzzy; too late and the team has moved on.
- **Attendee Selection**: Include all responders, the IC, affected service owners, and an optional facilitator who was not involved in the incident. Keep the group small enough for productive discussion (6-10 people).
- **Pre-Reading**: Distribute the incident timeline, metrics, and a draft postmortem document before the meeting so attendees arrive prepared.

### Meeting Structure
- **Time-Boxing**: Limit the meeting to 60 minutes. Allocate time explicitly: 10 minutes for timeline review, 20 minutes for root cause analysis, 20 minutes for action items, 10 minutes for wrap-up.
- **Blameless Facilitation**: The facilitator actively redirects blame-oriented language. Replace "X should have" with "What systemic condition allowed this?" Focus on processes, tooling, and system design rather than individual actions.
- **Timeline Walk-Through**: Walk through the incident timeline chronologically. At each decision point, ask what information was available at the time and whether the decision was reasonable given that information.

## 2. Root Cause Analysis

### Five Whys Technique
- **Iterative Questioning**: Ask "Why?" repeatedly to move from the immediate symptom to the underlying systemic cause. Stop when you reach a cause that can be addressed by a process or system change.
- **Multiple Root Causes**: Most incidents have contributing factors, not a single root cause. Identify 2-4 contributing factors and address each with targeted action items.
- **Avoid Stopping at Human Error**: "Human error" is never a root cause — it is a starting point. Ask why the system allowed the error to have impact (missing guardrails, confusing UI, inadequate testing).

### Contributing Factor Categories
- **Process Gaps**: Missing or outdated runbooks, unclear ownership, insufficient review processes.
- **Tooling Deficiencies**: Missing alerts, inadequate monitoring coverage, slow deployment pipelines that delay rollbacks.
- **System Design**: Single points of failure, missing circuit breakers, insufficient capacity headroom, lack of graceful degradation.

## 3. Action Item Quality

### Quality Criteria
- **Specific**: Each action item describes a concrete deliverable, not a vague improvement area. "Add connection pool exhaustion alert at 80% threshold" is specific; "improve monitoring" is not.
- **Assigned**: Every action item has a named owner responsible for completion, not a team or group.
- **Time-Bound**: Set a realistic due date based on priority. SEV1-related action items should have aggressive timelines; lower-severity items can be scheduled into normal sprint work.
- **Systemic**: Action items should address systemic causes, not just the specific failure instance. Prefer "add integration tests for all payment flows" over "add test for the specific failing case."

## 4. Organizational Learning

### Learning Repository
- **Searchable Archive**: Maintain a searchable repository of postmortem documents. Tag postmortems by failure category (capacity, deployment, dependency, configuration) to enable pattern analysis.
- **Trend Analysis**: Review postmortems quarterly to identify recurring themes. If the same failure category appears repeatedly, elevate it as a strategic engineering investment.

### Feedback Loop
- **Action Item Tracking**: Track postmortem action item completion rates. Low completion rates indicate either unrealistic action items or insufficient prioritization.
- **Roadmap Integration**: Feed postmortem findings into the engineering roadmap. Reliability improvements should compete for prioritization alongside feature work based on their impact on error budgets.
