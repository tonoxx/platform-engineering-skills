---
name: cloud-native-platform-engineering
description: Guide for Platform Engineers (PfE) to design, build, and operate cloud-native platforms (OpenShift/AWS). Use this skill for Environment as a Service (DevContainer/MCP), Golden Path creation, Day 0/1/2 operations, CI/CD, Observability, and DevSecOps.
license: MIT
---

# Cloud Native Platform Engineering

This skill provides comprehensive guidance for Platform Engineers (PfE) to design, build, and operate cloud-native platforms based on Platform Engineering, Site Reliability Engineering (SRE), and DevOps best practices. It focuses heavily on providing **Environment as a Service (EaaS)** using DevContainers and Model Context Protocol (MCP), specifically tailored for Kubernetes, Red Hat OpenShift, and Amazon Web Services (AWS).

## Core Principles for Platform Engineers

1. **Environment as a Service (EaaS)**: Provide ephemeral, fully configured development environments (DevContainers) via the Internal Developer Platform (IDP).
2. **Golden Paths**: Build "paved roads" (standardized, automated workflows via Backstage/OpenShift Templates) rather than enforcing strict mandates.
3. **AI-Empowered Autonomy**: Integrate MCP Servers into DevContainers to give AI agents (like Claude Code) secure, RBAC-compliant access to infrastructure and tools.
4. **AI and IaC Role Division**: In production environments, prioritize IaC for reproducibility and safety. Avoid using AI indiscriminately for direct production changes. Instead, extensively leverage AI for preparation tasks, standardization, code generation, and incident analysis prior to production deployment.

## Platform Lifecycle Operations (Day 0 / Day 1 / Day 2)

Platform Engineering is a continuous lifecycle. Determine the current operational phase and consult the appropriate reference file.

### 1. Day 0: Design & Architecture (Environment as a Service)
**Focus**: Designing the IDP, Golden Paths, and DevContainer architectures.
**Use when**: Setting up Backstage, configuring OpenShift Dev Spaces, designing DevContainer specifications, or establishing MCP Server integrations.
**Read**: `references/environment-as-a-service.md`

### 2. Day 1: Build & Deploy (Release Engineering & DevSecOps)
**Focus**: Automating software delivery and securing the supply chain.
**Use when**: Building CI/CD pipelines (Tekton/GitHub Actions), implementing GitOps (Argo CD), writing IaC (Terraform/Crossplane), or configuring Zero Trust (SPIFFE/SPIRE, Sigstore).
**Read**: `references/release-engineering-and-security.md`

### 3. Day 2: Operate & Optimize (SRE & Observability)
**Focus**: Ensuring reliability, managing incidents, and optimizing costs (FinOps).
**Use when**: Defining SLOs/SLIs, setting up OpenTelemetry/Prometheus, managing Toil Budgets, or implementing AIOps for incident response.
**Read**: `references/site-reliability-and-operations.md`

## Templates

When generating documentation, utilize the provided templates to ensure consistency:

- **SLO Definition**: Use `templates/slo_document.md` when defining Service Level Objectives.
- **Incident Postmortem**: Use `templates/postmortem.md` for blameless incident reviews.
- **Architecture Decision Record**: Use `templates/architecture_decision_record.md` when documenting significant architectural choices.
