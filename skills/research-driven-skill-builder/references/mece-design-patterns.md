# MECE Design Patterns for Skills

This document provides patterns for structuring a skill's knowledge base using MECE (Mutually Exclusive, Collectively Exhaustive) principles.

## Why MECE?

When an AI agent reads a skill, overlapping information causes confusion and wastes the context window. Missing information leads to hallucinations or task failure. MECE ensures the skill is both efficient and comprehensive.

## Common MECE Patterns for Skills

### 1. The Lifecycle Pattern (Time-based)
Best for operational or engineering skills where tasks follow a chronological order.

**Example: Platform Engineering**
- **Day 0 (Design & Architecture)**: Planning, IDP design, DevContainer specs.
- **Day 1 (Build & Deploy)**: CI/CD, IaC, Security integration.
- **Day 2 (Operate & Optimize)**: Observability, SRE, FinOps, Incident response.

### 2. The Layered Architecture Pattern (Stack-based)
Best for software development or system architecture skills.

**Example: Web Application Development**
- **Frontend**: UI components, state management, routing.
- **Backend**: API design, business logic, authentication.
- **Data Layer**: Database schema, ORM, caching.
- **Infrastructure**: Deployment, scaling, CDN.

### 3. The Persona/Role Pattern (Actor-based)
Best for organizational or process-oriented skills.

**Example: Agile Project Management**
- **Product Owner**: Backlog grooming, user story creation, prioritization.
- **Scrum Master**: Sprint planning, blocker removal, retrospective facilitation.
- **Developer**: Task estimation, implementation, code review.

### 4. The Capability Pattern (Feature-based)
Best for tool-specific or API-wrapper skills.

**Example: PDF Manipulation Skill**
- **Extraction**: Reading text, extracting images, parsing tables.
- **Modification**: Merging, splitting, rotating pages.
- **Generation**: Creating from HTML, adding watermarks, filling forms.

## How to Apply the Pattern

1. **Choose the Pattern**: Select the pattern that best fits the domain.
2. **Create Reference Files**: Create one Markdown file in the `references/` directory for each category in your MECE structure (e.g., `references/day-0-design.md`, `references/day-1-build.md`).
3. **Route in SKILL.md**: In the main `SKILL.md` file, clearly explain *when* the AI agent should read each reference file based on the user's current context.

**Example Routing in SKILL.md:**
```markdown
Determine the current operational phase and consult the appropriate reference file:
- If designing the architecture, read `references/day-0-design.md`.
- If building the CI/CD pipeline, read `references/day-1-build.md`.
- If responding to an incident, read `references/day-2-operate.md`.
```
