# Version Upgrades & Migration

This document covers strategies for upgrading IaC tool versions, migrating between IaC frameworks, and importing existing infrastructure.

## 1. Terraform Version Upgrades

### Upgrade Workflow
- **Read the Changelog**: Before upgrading, review the Terraform and provider changelogs for breaking changes, deprecated features, and required migration steps.
- **State Format Migration**: Major Terraform version upgrades may require state format migration. Always back up the state file before running terraform init with the new version.
- **Provider Version Bumps**: Upgrade providers independently from the Terraform core binary. Test provider upgrades in a non-production workspace first and review the plan output for unexpected resource changes.

### Testing Strategy
- **Plan-Only Validation**: Run terraform plan with the upgraded version against all environments to detect breaking changes before applying.
- **Incremental Upgrades**: For multi-version jumps, upgrade through each intermediate version sequentially rather than skipping versions.
- **Module Compatibility**: Verify that all referenced modules are compatible with the target Terraform and provider versions before upgrading the root configuration.

## 2. Cross-Tool Migration

### Terraform to Crossplane
- **Identify Migration Candidates**: Start with stateless or easily recreatable resources. Migrate complex stateful resources (databases, storage) last.
- **Resource Import**: Use Crossplane's observe-only policy to import existing cloud resources without modifying them, then transition to full management.
- **Gradual Cutover**: Run Terraform and Crossplane in parallel during migration, transferring resource ownership one resource group at a time.

### Terraform to Pulumi
- **Automated Conversion**: Use the pulumi convert tool to translate Terraform HCL to Pulumi programs. Review the generated code for language-specific improvements.
- **State Import**: Use pulumi import to bring existing resources under Pulumi management without recreating them. Verify the imported state matches the actual infrastructure.

## 3. Brownfield Import Strategies

### Import Workflow
- **Discovery**: Inventory existing cloud resources using provider-specific tools (AWS Config, GCP Asset Inventory) to identify resources not yet managed by IaC.
- **Selective Import**: Import resources in dependency order — networking first, then compute, then application-level resources — to maintain referential integrity in state.
- **Post-Import Validation**: After importing, run a plan to verify that the IaC configuration matches the actual resource configuration. Resolve any drift before proceeding.
