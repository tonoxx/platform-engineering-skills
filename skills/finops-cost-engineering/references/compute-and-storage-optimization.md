# Compute & Storage Optimization

This document covers strategies for reducing cloud spend through commitment purchases, Spot instances, rightsizing, and waste elimination.

## 1. Commitment Purchases

### AWS Reserved Instances vs Savings Plans
- **Reserved Instances (RIs)**: Commit to a specific instance type in a specific region for 1 or 3 years. Best for highly predictable, steady-state workloads where the instance family is unlikely to change.
- **Savings Plans**: Commit to a dollar-per-hour spend level rather than a specific instance type. Compute Savings Plans offer flexibility across instance families, regions, and even between EC2 and Fargate.
- **Decision Framework**: Use Savings Plans as the default for flexible workloads. Reserve RIs for workloads with known, fixed instance requirements where the deeper discount justifies the reduced flexibility.

### GCP Committed Use Discounts
- **Resource-Based CUDs**: Commit to a specific amount of vCPU and memory in a region. Suitable for workloads with predictable baseline compute needs.
- **Spend-Based CUDs**: Commit to a dollar amount of spend on specific services. Provides flexibility similar to AWS Savings Plans.

## 2. Spot and Preemptible Strategies

### Kubernetes Integration
- **Karpenter**: Use Karpenter on EKS to automatically provision Spot instances based on pod scheduling requirements. Karpenter selects from multiple instance types to maximize availability and minimize cost.
- **GKE Autopilot**: GKE Autopilot automatically manages node provisioning including Spot pod support, eliminating the need for manual node pool management.
- **Workload Suitability**: Spot instances are suitable for stateless, fault-tolerant workloads (batch jobs, CI runners, stateless web servers). Avoid Spot for stateful workloads, leader-elected systems, or single-replica deployments.

### Interruption Handling
- **Graceful Shutdown**: Configure preStop hooks and terminationGracePeriodSeconds to handle Spot interruption notices and drain workloads cleanly.
- **Pod Disruption Budgets**: Set PodDisruptionBudgets to ensure minimum availability during Spot reclamation events.

## 3. Rightsizing and Waste Elimination

### Rightsizing Analysis
- **Goldilocks**: Deploy Goldilocks with VPA in recommendation-only mode to generate right-sized resource request suggestions for each workload based on actual usage.
- **AWS Compute Optimizer**: Use Compute Optimizer to analyze EC2, EBS, and Lambda utilization and receive downsizing or instance family migration recommendations.

### Idle Resource Detection
- **Orphaned Resources**: Regularly scan for unattached EBS volumes, unused Elastic IPs, idle load balancers, and empty security groups. Automate cleanup with scheduled Lambda functions or Kubernetes CronJobs.
- **Non-Production Scheduling**: Implement automated start/stop schedules for development and staging environments during off-hours and weekends to eliminate idle compute costs.
