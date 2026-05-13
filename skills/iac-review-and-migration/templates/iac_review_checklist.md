# IaC Pull Request Review Checklist

## Change Overview
**PR Title**: [e.g., "Add RDS instance for payments service"]
**Author**: [Name]
**IaC Tool**: [Terraform / Crossplane / Pulumi]
**Target Environment**: [e.g., staging, production]

## Blast Radius Assessment
- **Resources Created**: [List new resources, e.g., "1x RDS instance, 1x Security Group"]
- **Resources Modified**: [List modified resources and what changes]
- **Resources Destroyed**: [List resources that will be deleted — highlight if any are stateful]
- **Affected Environments**: [e.g., staging only / all environments]

## Security Review
| Check | Status | Notes |
|---|---|---|
| No secrets or credentials in plain text | [Pass/Fail] | [e.g., "Using AWS Secrets Manager"] |
| IAM permissions follow least privilege | [Pass/Fail] | [e.g., "Role scoped to specific S3 bucket"] |
| Encryption enabled for data at rest | [Pass/Fail] | |
| Network access restricted appropriately | [Pass/Fail] | [e.g., "Security group allows only VPC CIDR"] |

## Cost Impact
- **Estimated Monthly Cost Change**: [e.g., "+$150/month for db.r6g.large"]
- **Cost Optimization Considered**: [e.g., "Reserved Instance eligible after validation period"]

## State Impact
- **State Operations Required**: [None / Import / Move / Remove]
- **State Backup Taken**: [Yes/No/N/A]
- **Migration Steps Documented**: [Yes/No/N/A]

## Rollback Plan
- **Rollback Strategy**: [e.g., "Revert PR and apply previous state" / "Destroy new resources only"]
- **Data Loss Risk**: [None / Possible — describe mitigation]
- **Estimated Rollback Time**: [e.g., "< 15 minutes"]

## Reviewer Sign-Off
- **Reviewer**: [Name]
- **Date**: [YYYY-MM-DD]
- **Decision**: [Approved / Approved with Comments / Request Changes]
