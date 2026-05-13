# TechDocs Authoring

## 1. TechDocs Architecture

- **Docs-as-Code**: Documentation lives alongside source code, versioned and
  deployed through the same CI/CD pipeline as the component it describes.
- **MkDocs Foundation**: TechDocs uses MkDocs with the Material theme to
  convert Markdown into a navigable site rendered inside Backstage.
- **Three-Stage Pipeline**: Generation (build), publication (upload), and
  serving (read) are configured independently for flexibility.

## 2. Setup and Configuration

- **Generator Options**: Choose local generation on the backend or external
  generation in CI. External is recommended at scale.
- **Publisher Backends**: Store generated sites in cloud object storage such
  as S3, GCS, or Azure Blob for durable and scalable hosting.
- **Caching**: Enable a CDN or cache layer in front of the publisher to
  reduce latency for frequently accessed documentation pages.

## 3. Writing Effective TechDocs

### Markdown Conventions

- **Heading Hierarchy**: Use a single H1 for the page title; organize content
  with H2 and H3 sections without skipping levels.
- **Admonitions**: Use admonition blocks for warnings, notes, and tips to
  highlight critical information without disrupting prose flow.
- **Linking Strategy**: Prefer relative links between pages within the same
  component to ensure portability across environments.

### Navigation and Diagrams

- **mkdocs.yml Nav**: Define explicit navigation trees rather than relying on
  automatic discovery to control the reader experience.
- **Mermaid Integration**: Use Mermaid syntax for architecture and sequence
  diagrams so they render natively without external image dependencies.
- **Image Assets**: Store images in a docs/assets directory and reference them
  with relative paths.

## 4. CI Integration

- **Build Validation**: Run a TechDocs build on every pull request to catch
  broken links, missing assets, and syntax errors before merge.
- **Automated Publishing**: Trigger publish on merge to the default branch so
  documentation stays current with the latest codebase changes.

## 5. Multi-Repo Documentation Aggregation

- **Entity Annotation**: Each catalog entity declares its TechDocs source via
  the backstage.io/techdocs-ref annotation for discovery and build.
- **Centralized vs Distributed**: Both monorepo and distributed docs models
  are supported transparently through annotation-based discovery.
- **Cross-Component Linking**: Use catalog entity references to create links
  between documentation sites, enabling a connected docs graph.
