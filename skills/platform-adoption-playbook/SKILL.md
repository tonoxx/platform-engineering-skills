---
name: platform-adoption-playbook
description: Internal platform adoption strategy, DORA metrics measurement, and Team Topologies application for driving platform engineering success. Use this skill when planning platform rollout, measuring engineering effectiveness, or restructuring teams around platform capabilities.
license: MIT
---

# Platform Adoption Playbook

This skill provides a structured approach to driving internal platform adoption, measuring engineering effectiveness, and organizing teams for platform success. It follows a Measure-Enable-Scale lifecycle that ensures data-driven adoption decisions.

## Core Principles

1. **Adoption is Earned, Not Mandated**: Internal platforms succeed through developer experience quality, not organizational mandates. If the platform is harder than the alternative, developers will route around it.
2. **Measure Outcomes, Not Outputs**: Track DORA metrics (deployment frequency, lead time, change failure rate, time to restore) to measure actual engineering effectiveness, not vanity metrics like lines of code or tickets closed.
3. **Team Topology as Architecture Input**: Organize platform teams as enabling teams (per Team Topologies), not gatekeeping teams. The platform team's success metric is how quickly stream-aligned teams can ship independently.
4. **Incremental Migration Over Big Bang**: Roll out platform capabilities incrementally to early-adopter teams, gather feedback, iterate, and expand. Never attempt a full-organization migration in a single phase.

## Adoption Lifecycle

Determine the current phase and consult the appropriate reference file.

### 1. Measure: DORA Metrics
**Focus**: Establishing baseline measurements of engineering effectiveness to guide platform investments.
**Use when**: Setting up DORA metric collection, benchmarking against industry data, or connecting engineering metrics to business outcomes.
**Read**: `references/dora-metrics-and-measurement.md`

### 2. Enable: Team Topologies
**Focus**: Structuring teams to maximize platform effectiveness and minimize cognitive load.
**Use when**: Defining platform team boundaries, choosing team interaction modes, assessing cognitive load, or identifying anti-patterns in team structure.
**Read**: `references/team-topologies-application.md`

### 3. Scale: Adoption Strategy & Rollout
**Focus**: Planning and executing the rollout of platform capabilities to the broader organization.
**Use when**: Identifying pilot teams, designing feedback loops, creating migration playbooks, or measuring adoption progress.
**Read**: `references/adoption-strategy-and-rollout.md`

## Templates

- **Adoption Plan**: Use `templates/adoption_plan.md` when planning the rollout of a new platform capability.
