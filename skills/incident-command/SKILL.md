---
name: incident-command
description: On-call rotation design, Incident Commander workflows, escalation criteria definition, and structured postmortem facilitation. Use this skill when designing incident management processes or commanding an active incident.
license: MIT
---

# Incident Command

This skill provides structured guidance for the entire incident management lifecycle — from on-call preparation through active incident response to post-incident learning. It focuses on the human processes that complement technical observability and automation.

## Core Principles

1. **Clear Roles, Not Heroes**: Every incident must have an explicitly assigned Incident Commander, Communications Lead, and Subject Matter Experts. Heroic individual effort indicates a process failure.
2. **Escalate Early, Escalate Structurally**: Define escalation criteria in advance (severity thresholds, time-based triggers, customer-impact thresholds). Delayed escalation is the most common cause of extended incidents.
3. **Blameless by Design**: Postmortem processes must be structurally blameless — focus language on systems and processes, not individuals. This is enforced through facilitation technique, not just policy statements.
4. **Practice Under Calm**: Incident response muscles atrophy without exercise. Run regular game days and tabletop exercises to validate runbooks and build team confidence before real incidents occur.

## Incident Lifecycle

Determine the current phase and consult the appropriate reference file.

### 1. Preparation: On-Call & Escalation Design
**Focus**: Building the organizational structure and processes that enable effective incident response.
**Use when**: Designing on-call rotations, defining severity levels, establishing escalation paths, or setting up alerting integrations.
**Read**: `references/on-call-and-escalation-design.md`

### 2. Response: Incident Commander Workflow
**Focus**: Coordinating the response to an active incident effectively.
**Use when**: Commanding an active incident, coordinating parallel workstreams, communicating status to stakeholders, or managing war room dynamics.
**Read**: `references/incident-commander-workflow.md`

### 3. Learning: Postmortem & Improvement
**Focus**: Extracting organizational learning from incidents and driving systemic improvements.
**Use when**: Facilitating a postmortem meeting, writing a postmortem document, evaluating action item quality, or building an organizational learning repository.
**Read**: `references/postmortem-and-learning.md`

## Templates

- **Severity Classification**: Use `templates/severity_classification.md` when defining incident severity levels and escalation criteria.
- **Incident Communication**: Use `templates/incident_communication.md` for status updates during an active incident.
