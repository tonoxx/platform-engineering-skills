# Scorecard Rubric for [Component Name]

- **Component**: [e.g., payments-service]
- **Owner**: [e.g., team-checkout]
- **Evaluation Date**: [e.g., 2026-Q2]
- **Target Level**: [e.g., Gold]

## Dimensions

### Documentation — Measurement: [e.g., Soundcheck TechDocs rule]

- **Bronze**: README exists. TechDocs annotation present in catalog entity.
- **Silver**: API spec current. Runbook covers top failure modes.
- **Gold**: ADRs maintained. Docs reviewed within the last quarter.

### Testing — Measurement: [e.g., CI coverage report via Soundcheck]

- **Bronze**: Unit tests run in CI on every pull request.
- **Silver**: Coverage meets threshold. Integration tests on critical paths.
- **Gold**: E2E tests in staging pre-deploy. Flaky rate below tolerance.

### Observability — Measurement: [e.g., SLO document in catalog metadata]

- **Bronze**: Structured logging. Health check endpoint exists.
- **Silver**: Tracing instrumented. Dashboard with key metrics.
- **Gold**: SLOs with error budgets. Alerts on burn rate.

### Security — Measurement: [e.g., vulnerability scanner API integration]

- **Bronze**: Dependency scanning in CI. No critical vulns past SLA.
- **Silver**: Secrets in vault. Container images signed.
- **Gold**: Latest security review passed. SBOM published.

### Operational Readiness — Measurement: [e.g., PagerDuty schedule check]

- **Bronze**: Ownership in catalog. Escalation path documented.
- **Silver**: On-call configured. Rollback tested within last quarter.
- **Gold**: DR plan exercised. Game day completed recently.

## Scoring: [composite weighted average or overall level]
