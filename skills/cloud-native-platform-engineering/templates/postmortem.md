# Incident Postmortem

**Incident Name**: [Brief, descriptive name]
**Date of Incident**: [YYYY-MM-DD]
**Authors**: [Names of people writing this document]
**Status**: [Draft / Under Review / Final]

## Executive Summary
[A brief, 1-2 paragraph summary of what happened, the impact, and the root cause. This should be understandable by non-technical stakeholders.]

## Impact
- **Duration**: [Start time to End time, Total duration]
- **User Impact**: [How were users affected? e.g., "Users could not complete checkout for 45 minutes."]
- **Revenue/Business Impact**: [Estimated financial or business impact, if applicable]

## Timeline
[A chronological list of events leading up to, during, and after the incident. Use UTC or a consistent timezone.]
- **[HH:MM]**: [Event description]
- **[HH:MM]**: [Event description]

## Root Cause Analysis (The 5 Whys)
[Ask "Why?" repeatedly until you reach the fundamental systemic issue.]
1. **Why did [the immediate problem] happen?** Because [Reason 1].
2. **Why did [Reason 1] happen?** Because [Reason 2].
3. **Why did [Reason 2] happen?** Because [Reason 3].
4. **Why did [Reason 3] happen?** Because [Reason 4].
5. **Why did [Reason 4] happen?** Because [Root Cause].

## What Went Well
[List things that worked as expected, e.g., monitoring caught the issue quickly, the team communicated effectively.]

## What Could Be Improved
[List areas for improvement, e.g., a specific alert was missing, a runbook was outdated, a single point of failure exists.]

## Action Items
[Specific, actionable tasks to prevent this incident from happening again or to mitigate its impact. Assign an owner and a deadline to each.]

| Action Item | Owner | Priority | Ticket/Issue Link |
|---|---|---|---|
| [e.g., Add alert for database connection pool exhaustion] | [Name] | [High/Medium/Low] | [Link] |
| [e.g., Update runbook for service restart] | [Name] | [High/Medium/Low] | [Link] |
