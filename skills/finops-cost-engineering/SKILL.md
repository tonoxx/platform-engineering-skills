---
name: finops-cost-engineering
description: Cloud cost analysis frameworks, Kubernetes cost allocation strategies, and Reserved/Spot optimization playbooks for AWS and GCP. Use this skill when performing cost reviews, building showback/chargeback models, or optimizing cloud spend.
license: MIT
---

# FinOps & Cost Engineering

This skill provides a structured approach to cloud cost management following the FinOps Foundation's Inform-Optimize-Operate lifecycle. It covers cost visibility, compute and storage optimization, and governance frameworks for AWS and GCP environments running Kubernetes workloads.

## Core Principles

1. **Unit Economics Over Totals**: Always express costs as a unit metric (cost per request, cost per customer, cost per namespace) rather than raw totals to enable meaningful comparison and budgeting.
2. **Allocation Before Optimization**: You cannot optimize what you cannot attribute. Establish namespace-level and team-level cost allocation before pursuing any optimization initiative.
3. **Commitment as a Spectrum**: Reserved Instances, Savings Plans, and Committed Use Discounts represent a commitment spectrum. Match the commitment level to workload predictability — never over-commit to volatile workloads.
4. **Waste Elimination is Continuous**: Orphaned resources, oversized instances, and idle capacity are recurring problems. Automate their detection and remediation as part of the operational cadence.

## FinOps Lifecycle

Determine the current phase of your FinOps practice and consult the appropriate reference file.

### 1. Visibility & Allocation (Inform)
**Focus**: Understanding where money is being spent and attributing costs to teams and services.
**Use when**: Setting up cost dashboards, implementing tagging strategies, or building showback/chargeback models.
**Read**: `references/cost-visibility-and-allocation.md`

### 2. Compute & Storage Optimization (Optimize)
**Focus**: Reducing cloud spend through rightsizing, commitment purchases, and waste elimination.
**Use when**: Evaluating Reserved Instance or Savings Plan purchases, implementing Spot strategies, or identifying idle resources.
**Read**: `references/compute-and-storage-optimization.md`

### 3. Governance & Budgets (Operate)
**Focus**: Establishing policies, budgets, and organizational processes to sustain cost efficiency.
**Use when**: Setting up budget alerts, enforcing tagging policies, creating executive cost reports, or maturing the FinOps practice.
**Read**: `references/finops-governance-and-budgets.md`

## Templates

- **Cost Optimization Proposal**: Use `templates/cost_optimization_proposal.md` when proposing a cost optimization initiative.
