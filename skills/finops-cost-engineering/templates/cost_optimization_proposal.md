# Cost Optimization Proposal

## Proposal Overview
**Title**: [e.g., "Migrate CI runners to Spot instances"]
**Author**: [Name]
**Date**: [YYYY-MM-DD]
**Priority**: [High / Medium / Low]

## Current State
- **Affected Resources**: [e.g., "20x m5.xlarge EC2 instances in us-east-1 running CI jobs"]
- **Current Monthly Spend**: [e.g., "$3,200/month"]
- **Utilization Rate**: [e.g., "Average CPU utilization: 25%, Peak: 70%"]
- **Cost Attribution**: [e.g., "Team: Platform, Service: CI/CD Pipeline"]

## Proposed Change
[Describe the optimization, e.g., "Replace on-demand CI runner instances with Spot instances using Karpenter, diversified across m5.xlarge, m5a.xlarge, and m6i.xlarge instance types"]

## Expected Savings
| Metric | Current | Projected | Savings |
|---|---|---|---|
| Monthly Compute Cost | [e.g., $3,200] | [e.g., $960] | [e.g., $2,240 (70%)] |
| Annual Savings | | | [e.g., $26,880] |

## Risk Assessment
- **Service Impact**: [e.g., "Spot interruptions may cause CI job retries, adding ~5% to average pipeline duration"]
- **Mitigation**: [e.g., "Configure graceful termination, job checkpointing, and PodDisruptionBudgets"]
- **Rollback Strategy**: [e.g., "Revert Karpenter NodePool to on-demand provisioner"]

## Implementation Plan
| Phase | Action | Timeline |
|---|---|---|
| 1 | [e.g., Deploy Karpenter with mixed Spot/on-demand NodePool] | [e.g., Week 1] |
| 2 | [e.g., Monitor Spot interruption rate and job success rate] | [e.g., Weeks 2-3] |
| 3 | [e.g., Increase Spot ratio to 80% if metrics are healthy] | [e.g., Week 4] |

## Approval
- **Approver**: [Name]
- **Decision**: [Approved / Rejected / Deferred]
- **Notes**: [Any conditions or follow-up actions]
