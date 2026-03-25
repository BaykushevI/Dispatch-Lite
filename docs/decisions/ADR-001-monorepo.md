# ADR-001: Monorepo for the Dispatch Lite baseline

## Status

Accepted

## Context

Dispatch Lite is a multi-component system with a UI, Orchestrator, Assignment Engine, Advisor, shared contracts, and shared documentation. Even though these components represent separate architectural boundaries, they are still part of a single portfolio project with one delivery roadmap and one evolving architecture baseline.

At this stage, the primary need is not independent team ownership or large-scale service autonomy. The primary need is clarity, consistency, and controlled evolution.

## Decision

Dispatch Lite will use a **monorepo structure** once repository bootstrap begins.

The monorepo will keep the main application, service components, shared contracts, sample data, and documentation in one repository, while still preserving logical separation between services.

## Consequences

This decision gives us:

- one place for architecture, contracts, and implementation
- easier coordination across shared changes
- simpler CI and validation setup
- better visibility into system evolution as a single portfolio artifact

This decision intentionally postpones:

- independent repository ownership per service
- separate release/version cycles
- service-level operational autonomy

The trade-off is that the repo will represent one system with multiple boundaries, rather than a set of independently managed codebases.

## Alternatives considered

- separate repository per service
- UI repo plus backend repo split
- single flat repo without service/package structure

## Follow-up notes

This decision supports:

- a shared contracts package
- a shared documentation set
- a single roadmap across all components

It also requires clear folder structure so that monorepo convenience does not blur service ownership.
