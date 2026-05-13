---
name: iac-review-and-migration
description: Code review checklists, version upgrade strategies, and state management best practices for Infrastructure as Code (Terraform, Crossplane, Pulumi). Use this skill when reviewing IaC pull requests or migrating between IaC tools or versions.
license: MIT
---

# IaC Review & Migration

This skill provides structured guidance for reviewing Infrastructure as Code, managing version upgrades, and performing safe state operations. It covers Terraform, Crossplane, and Pulumi with a focus on code quality, migration safety, and state integrity.

## Core Principles

1. **Blast Radius Scoping**: Every IaC change must have its blast radius explicitly assessed — which resources are created, modified, or destroyed, and in which environments.
2. **State Integrity First**: Never perform a migration, import, or refactor without first verifying and backing up the state file. State corruption is the highest-severity IaC failure.
3. **Drift as Signal**: Treat infrastructure drift not as noise but as a diagnostic signal indicating unauthorized manual changes or inadequate IaC coverage.
4. **Review Before Apply**: All IaC changes require human-readable plan output review. Automate plan generation in CI but never automate apply to production without explicit approval gates.

## IaC Activity Domains

Identify the activity you are performing and consult the appropriate reference file.

### 1. Code Review
**Focus**: Evaluating IaC pull requests for correctness, security, and maintainability.
**Use when**: Reviewing Terraform modules, Crossplane Compositions, or Pulumi programs for merge readiness.
**Read**: `references/iac-code-review-checklists.md`

### 2. Version Upgrades & Migration
**Focus**: Upgrading IaC tool versions or migrating between IaC frameworks.
**Use when**: Bumping Terraform or provider versions, migrating from Terraform to Crossplane or Pulumi, or importing brownfield infrastructure.
**Read**: `references/version-upgrades-and-migration.md`

### 3. State Operations & Drift Management
**Focus**: Managing state files, performing state surgery, and detecting infrastructure drift.
**Use when**: Configuring state backends, moving or importing resources in state, or investigating drift between declared and actual infrastructure.
**Read**: `references/state-operations-and-drift.md`

## Templates

- **IaC Review Checklist**: Use `templates/iac_review_checklist.md` when reviewing an Infrastructure as Code pull request.
