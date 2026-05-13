---
name: inner-source-developer-portal
description: Backstage plugin development, TechDocs authoring, and Software Catalog Scorecard maturity evaluation for building and operating Internal Developer Portals. Use this skill when developing Backstage plugins, writing TechDocs, or assessing service maturity.
license: MIT
---

# Inner-Source Developer Portal

This skill covers the design, build, and operation of Internal Developer Portals built on Backstage. It is organized by capability area — plugin development, TechDocs authoring, and service maturity evaluation via Scorecards.

## Core Principles

1. **Portal as Product**: Treat the Internal Developer Portal as an internal product with its own roadmap, user research, and adoption metrics. A portal nobody uses is worse than no portal at all.
2. **Documentation-Adjacent Discovery**: TechDocs should be the single source of truth for every cataloged component. If documentation is not discoverable alongside the service in the portal, it effectively does not exist.
3. **Maturity as Incentive, Not Mandate**: Use Scorecards to visualize service maturity (documentation completeness, test coverage, SLO existence) as positive incentives, not as punitive gates.

## Capability Areas

Identify the capability area and consult the appropriate reference file.

### 1. Backstage Plugin Development
**Focus**: Designing, scaffolding, and maintaining frontend and backend Backstage plugins.
**Use when**: Building new plugins, integrating external systems into the catalog, or configuring authentication and authorization for plugins.
**Read**: `references/backstage-plugin-development.md`

### 2. TechDocs Authoring
**Focus**: Writing, publishing, and aggregating docs-as-code documentation using TechDocs.
**Use when**: Setting up TechDocs infrastructure, writing effective documentation, configuring CI-based doc builds, or aggregating docs across repositories.
**Read**: `references/techdocs-authoring.md`

### 3. Scorecard & Maturity Evaluation
**Focus**: Defining and measuring service maturity dimensions using Backstage Scorecards.
**Use when**: Defining maturity rubrics, implementing scoring systems, tracking maturity trends, or gamifying quality improvements.
**Read**: `references/scorecard-maturity-evaluation.md`

## Templates

- **Scorecard Rubric**: Use `templates/scorecard_rubric.md` when defining a service maturity Scorecard with dimensions and scoring levels.
