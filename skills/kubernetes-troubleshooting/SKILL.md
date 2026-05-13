---
name: kubernetes-troubleshooting
description: Structured diagnosis and resolution workflows for Kubernetes failure patterns including Pod lifecycle errors, network policy misconfigurations, resource exhaustion, and storage issues. Use this skill when debugging a specific Kubernetes cluster problem.
license: MIT
---

# Kubernetes Troubleshooting

This skill provides structured diagnostic workflows for identifying and resolving common Kubernetes cluster failures. It is organized by failure domain to enable rapid root cause analysis based on observable symptoms.

## Core Principles

1. **Symptom-First Diagnosis**: Always start from the observable symptom (Pod status, event message, log output) and follow a structured decision tree to the root cause.
2. **Blast Radius Awareness**: Before applying any fix, assess whether the issue is node-scoped, namespace-scoped, or cluster-wide to avoid cascading remediation failures.
3. **Non-Destructive Investigation**: Prefer read-only diagnostic commands (describe, logs, get events) before any mutating operations (delete, patch, drain).
4. **Runbook Reproducibility**: Document every troubleshooting session as a reusable runbook so the same failure pattern never requires ad-hoc investigation twice.

## Failure Domains

Identify the primary symptom and consult the appropriate reference file.

### 1. Pod Lifecycle Failures
**Focus**: Pods that fail to start, crash repeatedly, or get evicted.
**Use when**: Encountering CrashLoopBackOff, ImagePullBackOff, OOMKilled, Pending pods, or Init container failures.
**Read**: `references/pod-lifecycle-failures.md`

### 2. Networking & DNS
**Focus**: Connectivity failures between services, DNS resolution errors, and traffic routing issues.
**Use when**: Services cannot reach each other, DNS lookups fail, Ingress returns errors, or NetworkPolicies block expected traffic.
**Read**: `references/networking-and-dns.md`

### 3. Storage & Resource Exhaustion
**Focus**: Persistent volume issues, disk pressure, and compute resource quota violations.
**Use when**: PVCs remain in Pending state, nodes report disk pressure, Pods are rejected due to quota limits, or nodes become NotReady.
**Read**: `references/storage-and-resource-exhaustion.md`

## Templates

- **Troubleshooting Runbook**: Use `templates/troubleshooting_runbook.md` when documenting a diagnosis and resolution for future reference.
