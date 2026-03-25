# ADR-004: No maps API in the initial architecture

## Status

Accepted

## Context

Dispatch Lite models delivery assignment and prioritization, but its goal is not to reproduce real-world routing complexity. Using an external maps API would add network dependencies, variable behavior, API cost, and non-deterministic routing input.

That would shift the project away from its core architectural purpose and toward infrastructure and integration concerns too early.

## Decision

Dispatch Lite will **not use a maps API** in the MVP or early implementation phases.

Instead, the system will rely on a predefined route and ETA matrix as part of the controlled operational input.

## Consequences

This decision gives us:

- deterministic route and ETA behavior
- easier testing and reproducibility
- lower setup and operational complexity
- stronger focus on architecture, prioritization, and explainability

This decision intentionally postpones:

- real-world routing integration
- dynamic traffic-aware ETA logic
- external dependency management for routing

The trade-off is reduced realism in route calculation. That is acceptable because the project is focused on system structure and decision reasoning, not logistics simulation accuracy.

## Alternatives considered

- direct integration with a maps provider
- mocked maps API behavior
- partial external routing plus internal fallback

## Follow-up notes

This decision supports:

- a stable CSV-first route matrix
- controlled dispatch scenarios
- cleaner reasoning and explanation outputs

It also makes future expansion possible if route realism ever becomes a deliberate next-phase requirement.
