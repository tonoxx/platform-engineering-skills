# DORA Metrics & Measurement

This document covers the four DORA metrics, measurement instrumentation, benchmarking, and connecting engineering metrics to business outcomes.

## 1. The Four DORA Metrics

### Throughput Metrics
- **Deployment Frequency**: How often code is deployed to production. Higher frequency indicates smaller, lower-risk deployments and a healthy CI/CD pipeline. Measure per service, not per organization.
- **Lead Time for Changes**: The time from code commit to code running in production. This measures the efficiency of the entire delivery pipeline including code review, CI, testing, and deployment.

### Stability Metrics
- **Change Failure Rate**: The percentage of deployments that result in a degraded service requiring remediation (rollback, hotfix, patch). This measures deployment quality and the effectiveness of pre-production validation.
- **Time to Restore Service (MTTR)**: The time from the start of a service disruption to full recovery. This measures the organization's ability to detect, respond to, and resolve incidents.

## 2. Measurement Instrumentation

### Data Sources
- **CI/CD Pipeline Events**: Capture deployment timestamps, build durations, and deployment results from the CI/CD system. Use webhook events from GitHub Actions, Tekton, or Argo CD to feed the metrics pipeline.
- **Git Metadata**: Derive lead time from the first commit timestamp to the deployment timestamp. Tag deployments with the corresponding commit SHA to enable traceability.
- **Incident Tracking**: Link incident start and resolution timestamps to the causing deployment. Integrate with PagerDuty, Opsgenie, or the incident management system to automate change failure rate and MTTR calculation.

### Tool Options
- **Sleuth**: Provides automated DORA metric tracking by integrating with Git, CI/CD, and incident management tools. Offers team-level dashboards and trend analysis.
- **LinearB**: Combines DORA metrics with developer workflow analytics (PR cycle time, review wait time) for a broader engineering effectiveness view.
- **Custom Dashboards**: Build custom DORA dashboards using CI/CD event data exported to a data warehouse. Use Grafana or Looker for visualization and alerting on metric regressions.

## 3. Benchmarking and Targets

### Industry Benchmarks
- **Elite Performers**: Deploy on-demand (multiple times per day), lead time under one hour, change failure rate under 5%, MTTR under one hour.
- **High Performers**: Deploy between once per day and once per week, lead time between one day and one week, change failure rate 10-15%, MTTR under one day.
- **Medium/Low Performers**: Deploy between once per week and once per month (or less), lead time between one week and six months, change failure rate 16-45%, MTTR between one day and one month.

### Setting Improvement Targets
- **Baseline First**: Measure current performance for at least one quarter before setting improvement targets. Targets without baselines are arbitrary.
- **Incremental Improvement**: Aim to move one performance tier at a time rather than jumping from Low to Elite. Each tier shift requires different organizational and technical investments.
- **Business Outcome Linkage**: Connect DORA improvements to business metrics (time to market for features, incident cost reduction, developer satisfaction) to justify platform investment to leadership.
