# Incident Commander Workflow

This document covers the Incident Commander role, communication cadence, war room facilitation, and decision-making during active incidents.

## 1. Incident Commander Role

### Responsibilities
- **Coordination, Not Fixing**: The IC coordinates the response but does not perform hands-on debugging. Their focus is maintaining situational awareness, assigning tasks, and removing blockers.
- **Single Decision Authority**: The IC makes final decisions when the team disagrees on approach. This prevents decision paralysis during high-pressure situations.
- **Role Assignment**: Upon declaring an incident, the IC immediately assigns a Communications Lead (handles stakeholder and status page updates) and identifies Subject Matter Experts for the affected systems.

### IC Selection
- **Rotation-Based Assignment**: Pre-assign the IC role as part of the on-call rotation so that every incident has a designated IC without negotiation.
- **Skill Requirements**: ICs need strong communication and coordination skills, but do not need to be the most senior engineer. Effective IC work is a distinct skill that improves with practice.

## 2. Communication Cadence

### Internal Updates
- **Regular Intervals**: Provide updates to the response team at fixed intervals (every 15 minutes for SEV1, every 30 minutes for SEV2). State what is known, what is being investigated, and what help is needed.
- **War Room Channel**: Maintain a dedicated incident channel for all communication. Keep the channel focused on the incident — move tangential discussions to threads or separate channels.

### Stakeholder Updates
- **Business Stakeholders**: Send executive-level summaries at defined intervals. Include impact scope, estimated time to resolution, and any customer-facing actions taken.
- **Status Page**: Update the public status page within 10 minutes of confirming customer impact. Update with each significant change in status (investigating, identified, monitoring, resolved).

### Customer Communication
- **Proactive Notification**: Notify affected customers before they report the issue when possible. Proactive communication builds trust even during service failures.
- **Plain Language**: Write customer-facing updates in non-technical language focused on impact and expected resolution, not internal technical details.

## 3. War Room Facilitation

### Structure
- **Opening Assessment**: Begin by stating the known facts, current impact, and initial hypothesis. Assign investigation workstreams based on the most likely root cause candidates.
- **Parallel Workstreams**: Assign independent investigation tracks to different team members. The IC tracks progress across workstreams and reallocates resources as hypotheses are confirmed or eliminated.
- **Time-Boxing**: Set explicit time limits for investigation approaches. If a hypothesis has not yielded results within the time box, pivot to the next most likely cause.

## 4. Decision-Making Under Uncertainty

### Mitigation vs Root Cause
- **Mitigate First**: Prioritize restoring service over identifying root cause. Rollbacks, traffic shifts, and feature flag disabling are valid mitigation strategies even before the root cause is confirmed.
- **Reversible Actions**: Prefer reversible mitigation actions (rollback, scale up, failover) over irreversible ones (data deletion, schema migration). Document the rationale for any irreversible action taken during an incident.

### IC Role Transfer
- **Planned Handoff**: For incidents lasting more than two hours, plan an IC handoff. The outgoing IC provides a structured briefing covering current status, open workstreams, and pending decisions.
- **Handoff Documentation**: Update the incident timeline and channel topic during the handoff to ensure the incoming IC and the team have current situational awareness.
