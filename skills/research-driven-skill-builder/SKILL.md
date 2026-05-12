---
name: research-driven-skill-builder
description: Guide for creating high-quality, comprehensive skills through deep research and MECE (Mutually Exclusive, Collectively Exhaustive) design. Use this when asked to create a new skill based on best practices, academic research, or complex domain knowledge.
license: MIT
---

# Research-Driven Skill Builder

This skill provides a structured workflow for creating robust, comprehensive, and well-organized skills. It ensures that the resulting skill is grounded in deep research, structured logically using MECE principles, and optimized for the AI agent's context window.

## Core Philosophy

1. **Research First**: Never assume you know all the best practices. Always conduct deep, parallel research across official docs, academic papers, and industry reports.
2. **MECE Structure**: Organize the gathered knowledge into Mutually Exclusive, Collectively Exhaustive domains. This prevents overlap and ensures no critical area is missed.
3. **Progressive Disclosure**: Keep `SKILL.md` concise (under 500 lines). Use it as a router to load detailed `references/` only when needed.

## The Workflow

Building a research-driven skill involves these sequential steps:

### Step 1: Scope Definition & Initial Planning
1. Analyze the user's request to identify the core domain and target audience.
2. Create a multi-phase task plan (e.g., Research -> MECE Design -> Implementation -> Validation).

### Step 2: Deep Parallel Research
1. Identify 5-8 sub-topics within the domain.
2. Conduct deep research on each sub-topic using web search and documentation review.
3. For each sub-topic, gather:
   - Architectural patterns and best practices.
   - Specific implementation examples (e.g., commands, code snippets).
   - Real-world use cases.
   - Source URLs.
4. Read the aggregated research results (JSON/CSV).

### Step 3: MECE Design & Self-Review
1. Synthesize the research results.
2. Categorize the information into 3-5 MECE domains.
3. **Self-Review**: Critically evaluate the proposed structure against the user's original request. Are there any missing perspectives (e.g., Day 2 operations, security, specific tool integrations)?
4. If gaps are found, conduct targeted follow-up research before proceeding.

### Step 4: Skill Implementation
1. Create the skill directory structure: `<skill-name>/`, `<skill-name>/references/`, and optionally `<skill-name>/templates/`.
2. Write the `SKILL.md` file. It must contain the core principles and act as a router to the reference files based on the MECE domains.
3. Write the detailed reference files in `references/`. Each file should cover one MECE domain comprehensively.
4. Create reusable templates in `templates/` if applicable (e.g., document templates, configuration boilerplates).

### Step 5: Validation & Delivery
1. Verify the directory structure and ensure all referenced files exist.
2. Confirm that `SKILL.md` is under 500 lines and each reference file path is correct.
3. Deliver the completed skill to the user.

## Reference

For detailed guidance on structuring the output files, refer to:
- `references/mece-design-patterns.md`: Patterns for structuring information.
