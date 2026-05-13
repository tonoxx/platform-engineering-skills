# State Operations & Drift Management

This document covers state backend configuration, state surgery operations, drift detection, and state file security best practices.

## 1. State Backend Configuration

### Remote State Backends
- **AWS (S3 + DynamoDB)**: Store state in S3 with server-side encryption enabled. Use a DynamoDB table for state locking to prevent concurrent modifications.
- **GCP (GCS)**: Store state in a GCS bucket with object versioning enabled for state history and recovery. GCS provides built-in locking.
- **Terraform Cloud/Enterprise**: Provides remote state storage with built-in locking, run history, and access controls. Suitable for teams that need centralized state governance.

### Backend Security
- **Encryption at Rest**: Enable server-side encryption on the state storage bucket. State files contain sensitive information including resource IDs and sometimes credentials.
- **Access Control**: Restrict state file access to the minimum set of principals that need it. Use IAM policies to separate read and write access.

## 2. State Surgery Operations

### Common Operations
- **State Move (mv)**: Relocate a resource within state when refactoring module structures. This avoids destroying and recreating the resource.
- **State Remove (rm)**: Remove a resource from state management without destroying the actual infrastructure. Used when transferring ownership to another IaC configuration.
- **State Import**: Bring an existing cloud resource under IaC management by adding it to the state file and writing the corresponding configuration.
- **Taint and Untaint**: Mark a resource for forced recreation on the next apply (taint) or clear that mark (untaint). Use sparingly and prefer targeted replacement.

### Safety Procedures
- **Always Back Up State**: Before any state surgery, download a copy of the current state file. This enables recovery if the operation produces unexpected results.
- **Plan After Surgery**: Run a plan immediately after any state operation to verify that the resulting state matches the actual infrastructure with no unintended changes.

## 3. Drift Detection and Remediation

### Detection Workflows
- **Scheduled Plan Runs**: Run terraform plan on a schedule (daily or per-commit) to detect drift between the declared configuration and actual infrastructure.
- **Cloud-Native Drift Detection**: Use cloud provider tools (AWS Config Rules, GCP Security Command Center) to detect changes made outside of IaC.
- **Alerting**: Route drift detection results to the team's alerting channel. Classify drift by severity — security-relevant drift requires immediate remediation.

### Remediation Strategy
- **Re-Apply IaC**: For drift caused by manual changes, re-apply the IaC configuration to restore the declared state. Communicate with the team to understand why the manual change was made.
- **Update IaC to Match**: If the manual change was intentional and correct, update the IaC configuration to reflect the new desired state and import the change.
- **Root Cause Analysis**: Investigate why drift occurred. Common causes include emergency manual fixes, insufficient IaC coverage, and lack of access controls on the cloud console.
