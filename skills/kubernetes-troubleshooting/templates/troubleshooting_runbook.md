# Troubleshooting Runbook

## Incident Overview
**Symptom**: [Describe the observed symptom, e.g., "Pods in namespace X are in CrashLoopBackOff"]
**Date Identified**: [YYYY-MM-DD]
**Severity**: [e.g., SEV2 — Service degraded for subset of users]
**Author**: [Name of the person documenting this runbook]

## Affected Scope
- **Cluster**: [e.g., production-us-east-1]
- **Namespace**: [e.g., payments]
- **Affected Resources**: [e.g., Deployment/payment-service, 3/5 replicas affected]
- **Blast Radius**: [Node-scoped / Namespace-scoped / Cluster-wide]

## Diagnostic Steps

### Step 1: [e.g., Check Pod Status and Events]
- **Command/Action**: [Describe what to check or run]
- **Expected Output**: [What a healthy state looks like]
- **Observed Output**: [What was actually observed]

### Step 2: [e.g., Review Container Logs]
- **Command/Action**: [Describe what to check or run]
- **Expected Output**: [What a healthy state looks like]
- **Observed Output**: [What was actually observed]

### Step 3: [e.g., Verify Resource Limits and Node Capacity]
- **Command/Action**: [Describe what to check or run]
- **Expected Output**: [What a healthy state looks like]
- **Observed Output**: [What was actually observed]

## Root Cause
[Describe the identified root cause, e.g., "Memory limit set to 256Mi but application requires 512Mi under load"]

## Resolution
[Describe the steps taken to resolve the issue, e.g., "Increased memory limit to 512Mi and applied the updated Deployment manifest"]

## Prevention
[Describe what should be done to prevent recurrence]
- [e.g., Add memory usage alerting at 80% of limit threshold]
- [e.g., Update load testing to cover peak traffic scenarios]
- [e.g., Add this scenario to the team's troubleshooting knowledge base]
