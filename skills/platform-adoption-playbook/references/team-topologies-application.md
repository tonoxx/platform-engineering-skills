# Team Topologies Application

This document covers the four team types, interaction modes, cognitive load assessment, and anti-patterns in platform team organization.

## 1. The Four Team Types

### Stream-Aligned Teams
- **Definition**: Teams aligned to a single stream of business value (a product, a feature set, a user journey). They have end-to-end ownership from development through deployment to production operation.
- **Platform Relationship**: Stream-aligned teams are the primary consumers of the platform. The platform's success is measured by how effectively it enables these teams to deliver value independently.

### Platform Teams
- **Definition**: Teams that build and maintain the internal platform — the shared foundation of tools, services, and APIs that stream-aligned teams use to deliver.
- **Operating Model**: Platform teams should operate as internal product teams, treating stream-aligned teams as customers. They gather requirements, prioritize features, and measure adoption like an external product team.

### Enabling Teams
- **Definition**: Teams that help stream-aligned teams acquire new capabilities (e.g., adopting a new technology, improving testing practices, migrating to a new platform feature).
- **Engagement Model**: Enabling teams work with stream-aligned teams temporarily (weeks to months) to transfer knowledge, then disengage. Permanent dependency on an enabling team is an anti-pattern.

### Complicated-Subsystem Teams
- **Definition**: Teams responsible for a subsystem that requires deep specialist knowledge (e.g., ML model serving, real-time data processing, cryptographic services).
- **Boundary**: These teams own a well-defined API boundary. Stream-aligned teams consume the subsystem as a service without needing to understand its internal complexity.

## 2. Interaction Modes

### Collaboration
- **When to Use**: When two teams need to work closely together to discover or build something new. Collaboration is high-bandwidth but also high-cost — use it for time-bounded discovery, not ongoing work.
- **Duration**: Collaboration should be explicitly time-bounded (4-8 weeks). If collaboration persists beyond this window, reconsider team boundaries.

### X-as-a-Service
- **When to Use**: When one team provides a stable capability that another team consumes via a well-defined API or interface. This is the default interaction mode for platform teams serving stream-aligned teams.
- **Quality Bar**: The platform capability must be reliable, documented, and self-serviceable. If stream-aligned teams frequently need to file support tickets, the interface is not mature enough for X-as-a-Service.

### Facilitating
- **When to Use**: When an enabling team helps a stream-aligned team adopt a new practice or technology. The enabling team coaches and guides but does not do the work for the stream-aligned team.

## 3. Cognitive Load Assessment

### Assessment Approach
- **Cognitive Load Types**: Distinguish between intrinsic load (essential domain complexity), extraneous load (unnecessary complexity from tools and processes), and germane load (learning and improvement effort).
- **Platform Goal**: The platform should reduce extraneous cognitive load on stream-aligned teams. If teams spend more time wrestling with the platform than building features, the platform is adding load, not removing it.
- **Measurement**: Survey stream-aligned teams quarterly on perceived cognitive load. Track the ratio of time spent on platform-related tasks vs feature delivery as a proxy metric.

## 4. Anti-Patterns

### Common Mistakes
- **Platform as Gatekeeper**: The platform team controls deployments, approves changes, or mandates specific tools without offering self-service alternatives. This creates bottlenecks and resentment.
- **Too Many Collaboration Interactions**: Multiple teams in long-running collaboration mode indicates unclear boundaries. Resolve by clarifying ownership and establishing X-as-a-Service interfaces.
- **Oversized Platform Team**: A platform team that is larger than the stream-aligned teams it serves is likely doing work that should be owned by stream-aligned teams. Evaluate whether the platform team is absorbing responsibilities rather than enabling self-service.
