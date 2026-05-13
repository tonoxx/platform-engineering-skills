# Severity Classification Definition

## Purpose
[Describe the purpose of this severity classification, e.g., "Define consistent incident severity levels across all engineering teams to ensure appropriate response and escalation"]

## Severity Levels

### SEV1 — Critical
- **User Impact**: [e.g., "Complete service outage affecting all users"]
- **Revenue Impact**: [e.g., "Direct revenue loss exceeding $X/hour"]
- **Data Impact**: [e.g., "Data loss, corruption, or security breach"]
- **Response Time**: [e.g., "Immediate — all responders paged within 5 minutes"]
- **Escalation Path**: [e.g., "L1 -> L2 -> L3 + Engineering VP within 15 minutes"]
- **Communication Cadence**: [e.g., "Status page update every 15 minutes, executive update every 30 minutes"]

### SEV2 — Major
- **User Impact**: [e.g., "Significant degradation affecting >25% of users"]
- **Revenue Impact**: [e.g., "Measurable revenue impact or SLO breach"]
- **Data Impact**: [e.g., "No data loss but data processing delayed"]
- **Response Time**: [e.g., "Within 15 minutes during business hours, 1 hour outside"]
- **Escalation Path**: [e.g., "L1 -> L2 within 30 minutes if unresolved"]
- **Communication Cadence**: [e.g., "Status page update every 30 minutes"]

### SEV3 — Minor
- **User Impact**: [e.g., "Partial degradation affecting <10% of users or non-critical feature"]
- **Revenue Impact**: [e.g., "Minimal or no direct revenue impact"]
- **Data Impact**: [e.g., "None"]
- **Response Time**: [e.g., "Acknowledged within 1 business hour, resolved within 1 business day"]
- **Escalation Path**: [e.g., "L1 triage, escalate to L2 if unresolved by end of day"]
- **Communication Cadence**: [e.g., "Internal update only, no status page update"]

### SEV4 — Low
- **User Impact**: [e.g., "Cosmetic issue or minor inconvenience, workaround available"]
- **Revenue Impact**: [e.g., "None"]
- **Data Impact**: [e.g., "None"]
- **Response Time**: [e.g., "Addressed during normal sprint work"]
- **Escalation Path**: [e.g., "No escalation — tracked as a standard ticket"]
- **Communication Cadence**: [e.g., "None required"]

## Classification Guidelines
[Describe how to choose between severity levels when the impact is ambiguous, e.g., "When in doubt, classify one level higher and downgrade after assessment. It is safer to over-respond than under-respond."]

## Review Cadence
[e.g., "Review and update this classification quarterly or after any SEV1 incident that reveals gaps in the current definitions"]
