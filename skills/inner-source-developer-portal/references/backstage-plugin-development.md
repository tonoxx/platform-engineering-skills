# Backstage Plugin Development

## 1. Plugin Architecture

- **Frontend Plugins**: React components registered via the app, owning their
  routes, pages, and sidebar entries within the Backstage shell.
- **Backend Plugins**: Express routers exposing REST or GraphQL endpoints,
  accessing databases and external APIs through the plugin environment.
- **Plugin Isolation**: Plugins communicate through well-defined APIs and
  shared contexts, never by direct import of another plugin's internals.

## 2. Plugin Scaffolding

- **CLI Generator**: Use the Backstage CLI to scaffold new plugins with the
  recommended directory layout, dependency wiring, and test setup.
- **Package Naming**: Follow the organization prefix convention to ensure
  discoverability in the monorepo or package registry.
- **Dev Server**: Run plugins in isolation using the standalone dev server
  before integrating into the full application.

## 3. Catalog Entity Model

- **Component**: A piece of software such as a service, library, or website
  that declares an owner and lifecycle stage.
- **API**: An interface exposed by a Component, including its type and spec.
- **Resource**: An infrastructure dependency such as a database or queue.
- **System**: A collection of Components and Resources delivering a capability.
- **Domain**: A grouping of Systems aligned to a business unit or product area.

## 4. Custom Entity Providers

- **Entity Provider Pattern**: Ingest catalog entities from external sources
  such as CMDBs, service registries, or cloud resource inventories.
- **Incremental Ingestion**: Design providers for incremental updates to
  reduce load on source systems and improve data freshness.

## 5. Integration with External Systems

- **Integrations Config**: Register source control hosts, CI/CD platforms,
  and cloud providers in the Backstage integrations configuration block.
- **Proxy Endpoints**: Tunnel frontend requests to authenticated external
  APIs via the proxy backend plugin without exposing secrets to the browser.

## 6. Authentication and Authorization

- **Identity Resolution**: Map user identity from auth providers to ownership
  references in catalog entities.
- **Permission Framework**: Define fine-grained policies controlling who can
  view, create, or modify catalog entities and plugin resources.
- **Token Scoping**: Validate and scope backend tokens to minimum required
  claims to prevent privilege escalation across plugins.
