# Cost Visibility & Allocation

This document covers cost attribution strategies, tagging conventions, and dashboard construction for cloud and Kubernetes environments.

## 1. Kubernetes Cost Allocation

### Namespace-Level Attribution
- **Kubecost**: Deploy Kubecost to attribute cluster costs (compute, memory, storage, network) down to the namespace, deployment, and pod level. Kubecost integrates with cloud billing data to provide blended costs.
- **OpenShift Cost Management**: For OpenShift clusters, use the built-in Cost Management operator to generate per-project cost reports and integrate with cloud provider billing.
- **Shared Cost Distribution**: Cluster-level costs (control plane, system DaemonSets, monitoring infrastructure) must be distributed across tenants using a fair-share model based on resource consumption or fixed allocation ratios.

### Showback vs Chargeback
- **Showback**: Report costs to teams for visibility without financial consequences. Suitable for organizations beginning their FinOps journey where cultural adoption matters more than enforcement.
- **Chargeback**: Allocate actual costs to team budgets. Requires mature tagging, accurate allocation, and organizational agreement on cost distribution methodology.

## 2. Cloud Provider Cost Tools

### AWS
- **Cost Explorer**: Use for interactive cost analysis with filtering by service, account, tag, and time period. Enable hourly granularity for investigating cost spikes.
- **Cost and Usage Reports (CUR)**: Export detailed billing data to S3 for custom analysis. CUR provides line-item granularity including resource IDs, pricing models, and usage quantities.
- **AWS Organizations**: Use consolidated billing across accounts to aggregate volume discounts and centralize cost governance.

### GCP
- **Billing Export to BigQuery**: Export billing data to BigQuery for SQL-based analysis. Enables custom dashboards, anomaly detection queries, and cross-project cost comparison.
- **Billing Budgets API**: Programmatically create and manage budgets across projects with threshold-based alerting.

## 3. Tagging Strategy

### Implementation
- **Mandatory Tags**: Define a minimum set of required tags (team, environment, service, cost-center) and enforce them via policy-as-code (AWS SCP, GCP Organization Policy, OPA Gatekeeper).
- **Tag Compliance Monitoring**: Track tag compliance rates and report untagged resources. Untagged resources are unallocatable costs that distort team-level reporting.
- **Automation**: Use tag propagation rules and default tags in IaC modules to ensure consistent tagging without relying on manual application.
