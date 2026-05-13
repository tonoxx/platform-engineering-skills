# IaC Code Review Checklists

This document provides structured review checklists for evaluating Infrastructure as Code pull requests across Terraform, Crossplane, and Pulumi.

## 1. Terraform Module Review

### Variable and Output Hygiene
- **Variable Validation**: Ensure input variables use validation blocks with meaningful error messages to catch invalid values before apply.
- **Output Exposure**: Verify that outputs do not inadvertently expose sensitive values. Mark sensitive outputs with the sensitive attribute.
- **Type Constraints**: Use precise type constraints (object, list, map) instead of generic any to catch type errors at plan time.

### Provider and Version Pinning
- **Provider Versions**: Pin provider versions with pessimistic constraints (e.g., ~> 5.0) to allow patch updates while preventing breaking changes.
- **Terraform Version**: Specify required_version to prevent accidental execution with incompatible Terraform versions.
- **Module Source Pinning**: Reference modules with exact version tags or commit SHAs rather than branch references to ensure reproducibility.

### Lifecycle and State Safety
- **Lifecycle Rules**: Verify that prevent_destroy is set on critical resources (databases, encryption keys) to guard against accidental deletion.
- **Create Before Destroy**: Use create_before_destroy for resources that cannot tolerate downtime during replacement (load balancers, DNS records).
- **Ignore Changes**: Apply ignore_changes only with clear justification, as it masks drift and can lead to configuration divergence.

## 2. Crossplane Composition Review

### XRD and Composition Validation
- **XRD Schema Completeness**: Verify that the CompositeResourceDefinition schema covers all required fields with appropriate types and descriptions.
- **Patch-and-Transform Correctness**: Review each patch to ensure source and destination field paths are correct and transforms produce valid values.
- **Ready Condition Mapping**: Ensure the Composition maps underlying resource readiness conditions to the composite resource's status.

## 3. Pulumi Program Review

### Language-Specific Patterns
- **Config vs Secrets Separation**: Verify that sensitive values use Pulumi secrets (Config.requireSecret) rather than plain config values.
- **Resource Naming**: Ensure resources use the auto-naming pattern or explicit physical names consistently to avoid naming conflicts across stacks.
- **Error Handling**: Review promise chains and async operations for proper error propagation, especially in TypeScript and Python programs.
