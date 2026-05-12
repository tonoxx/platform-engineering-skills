# Environment as a Service (EaaS) & Golden Paths

This document outlines the Day 0 operations for Platform Engineers: designing the Internal Developer Platform (IDP), establishing Golden Paths, and providing Environment as a Service (EaaS) using DevContainers and Model Context Protocol (MCP).

## 1. Environment as a Service (EaaS) Architecture

EaaS aims to eliminate the "it works on my machine" problem by providing ephemeral, fully configured development environments.

### DevContainer Specifications
- **Definition**: Use `devcontainer.json` to define the development environment, including the base image, required VS Code/Cursor extensions, and post-create commands.
- **Implementation**: Store the `devcontainer.json` in the application's Git repository. This ensures the environment is version-controlled alongside the code.

### Platform Implementations
- **OpenShift Dev Spaces**: A Kubernetes-native IDE and developer workspace platform. It uses `Devfile` (a YAML-based standard) to define workspaces that run as Pods within OpenShift. It integrates seamlessly with Red Hat Developer Hub (Backstage).
- **Coder / Gitpod**: Solutions that provision DevContainers on Kubernetes or cloud VMs (AWS EC2). Coder uses Terraform to define the underlying infrastructure for the workspaces.

### Security and Secret Injection
- **Zero Trust**: Workspaces should not have long-lived credentials.
- **Dynamic Secrets**: Integrate HashiCorp Vault or AWS Secrets Manager. Use init containers or Vault Agent sidecars to inject short-lived secrets into the DevContainer at runtime.

## 2. MCP Integration for Autonomous AI Agents

To enable AI agents (like Claude Code or Cursor) to perform autonomous tasks within the DevContainer, integrate Model Context Protocol (MCP) Servers.

### Architecture Pattern
1. **The DevContainer**: Acts as the secure boundary.
2. **MCP Proxy**: Because DevContainers often restrict standard I/O (stdio) communication, deploy an MCP Proxy within the container to translate stdio to Server-Sent Events (SSE) or HTTP.
3. **MCP Servers**: Run specific MCP servers within or alongside the DevContainer.

### Key MCP Servers for Platform Engineering
- **Kubernetes MCP Server**: Allows the AI agent to query cluster state (e.g., "Why is this Pod crashing?") using read-only RBAC permissions.
- **Terraform / AWS CDK MCP Server**: Enables the AI to read infrastructure state, validate IaC templates, and suggest modifications.
- **ArgoCD MCP Server**: Allows the AI to check deployment sync status and trigger rollbacks.

### Authorization Model
- **Least Privilege**: The MCP Server must run with a dedicated Service Account. If the AI agent is only meant to troubleshoot, the Kubernetes MCP Server must be configured with `read-only` RBAC roles.
- **Audit Logging**: Ensure all actions taken by the AI agent via the MCP Server are logged and attributable to the specific developer's session.

## 3. Golden Path Implementation

A Golden Path is an opinionated, supported, and automated workflow provided by the IDP.

### Building the Template Catalog
- **Backstage Software Templates**: Use Backstage's Scaffolder to create templates. A template typically uses `cookiecutter` or `copier` to generate a repository containing:
  - Application boilerplate (e.g., FastAPI, Spring Boot).
  - `devcontainer.json` or `Devfile` for EaaS.
  - CI/CD pipeline definitions (e.g., GitHub Actions, Tekton).
  - Kubernetes manifests or Helm charts.
- **OpenShift Templates**: Utilize OpenShift's native templating mechanism for quick instantiation of services within the cluster.

### Developer Onboarding
When a new developer joins, they log into the IDP, select a Golden Path template, and within minutes, they have a Git repository, a running DevContainer, and a CI/CD pipeline ready to deploy to a staging environment.
