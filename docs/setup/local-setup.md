# Local Setup

## Setup philosophy for Dispatch Lite

The local setup for Dispatch Lite is intentionally minimal.
The setup must not introduce architectural constraints.

The goal is to:

- reduce friction during early development
- avoid unnecessary infrastructure
- focus on architecture and contracts first
- enable fast iteration and testing

At this stage:

- no complex environments are required
- no container orchestration is needed
- no external services are required

The setup is designed to support:

- documentation-first development
- local service execution later (Workers-based)
- REST-based testing and validation

---

## Required tools

The following tools are required for Phase 0 and early Phase 1 work:

### Node.js (LTS)

- used as the base runtime environment
- required for future Workers development and tooling

### pnpm

- used for workspace and dependency management
- preferred for monorepo structure
- enables efficient dependency management across services and shared packages

### Git

- version control
- required for commit-based workflow

### VS Code

- primary development environment
- supports extensions and markdown workflows

### Wrangler CLI

- required for Cloudflare Workers development later
- used for local simulation and deployment workflows

### REST Client (VS Code extension)

- used for manual API testing
- enables request-based workflow without external tools

### ESLint

- ensures consistent code standards (used later)
- installed early as part of setup discipline

### Prettier

- ensures consistent formatting across files
- used for both code and markdown formatting

---

## Recommended tools

These tools are not strictly required but improve the development experience:

### Node version manager (nvm or Volta)

- ensures consistent Node version across environments
- prevents version drift

### GitLens (VS Code extension)

- improves visibility into git history and changes

### Markdown tooling (e.g., Markdown All in One)

- improves documentation authoring experience

### YAML support (VS Code extension)

- useful for configuration files later

### Error Lens (VS Code extension)

- improves visibility of errors and warnings

---

## Optional tools

These tools are not needed immediately but may be useful later:

### Docker (installed, not used initially)

Docker should be installed but not used in early phases.

- may be used later for:
  - Python Assignment Engine
  - local service simulation
- not required for Phase 0 or early implementation

### Diagram tooling (e.g., Draw.io, Mermaid preview)

- useful for visualizing architecture diagrams

### Terminal enhancements (optional)

- shell customization (zsh plugins, etc.)
- improves productivity but not required

---

## What we are intentionally not installing yet

The following are deliberately excluded from the setup:

- Python runtime (for Assignment Engine evolution)
- Kubernetes / K3s
- Raspberry Pi integration
- queue systems or local queue emulators
- database systems (Postgres, etc.)
- vector databases or retrieval systems
- observability stacks (Prometheus, Grafana, etc.)
- full Docker-based environments

These are deferred to later phases to avoid:

- unnecessary complexity
- premature infrastructure decisions
- setup overhead without immediate value

These components are deferred because they do not provide immediate architectural value in Phase 0.

---

## Local development assumptions

The setup assumes:

- development is performed on a **MacBook Pro (M4 Pro)**
- all work is done locally in a single environment
- no distributed or multi-machine setup is required
- all data is sourced from local CSV files
- services will initially run locally using lightweight tooling

The Raspberry Pi and other machines are not part of the development flow at this stage.
All components are expected to run in a single-process development model initially.

---

## Editor setup expectations

The development environment should support:

- consistent formatting (Prettier)
- linting (ESLint)
- markdown editing
- REST request execution

Minimum recommended VS Code extensions:

- REST Client
- ESLint
- Prettier
- Markdown All in One
- GitLens
- YAML
- Error Lens

The goal is to:

- keep the environment clean
- avoid unnecessary extensions
- ensure consistent behavior across files

Formatting and linting should be enforced automatically where possible.

---

## Environment management notes

To maintain consistency:

- use a fixed Node.js LTS version
- avoid mixing global and local package installations
- keep tooling versions consistent across environments
- ensure formatting and linting are applied consistently

No complex environment configuration is required at this stage.

No environment variables or secrets are needed yet.

Tooling should be reproducible across environments with minimal setup variance.

---

## Setup readiness checklist

Before starting implementation, confirm:

- Node.js (LTS) is installed and working
- pnpm is installed and available
- Git is configured and working
- VS Code is installed
- required extensions are installed:
  - REST Client
  - ESLint
  - Prettier
- Wrangler CLI is installed
- markdown files can be edited and previewed
- REST requests can be executed from VS Code
- no unnecessary tools or services are installed beyond the defined scope

If all items are complete, the environment is ready for:

- documentation work
- repo bootstrap (next phase)
- contract and service implementation setup
