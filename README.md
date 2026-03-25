# Dispatch Lite

## What this project is

Dispatch Lite is the final Level 3 project in a three-stage architecture roadmap focused on building real-world system design thinking.

It models a delivery dispatch and assignment domain through a deliberately structured system that separates coordination, decision-making, and explainability. The goal is not to simulate real-world logistics complexity, but to design a system that is clear, extensible, and architecturally sound.

This project is intentionally scoped to remain manageable while still reflecting how real distributed systems are designed and evolved.

## Why this project exists

Level 1 focused on modular monolith design.  
Level 2 introduced service extraction and asynchronous thinking.  
Level 3 focuses on distributed system boundaries, orchestration, and decision transparency.

Dispatch Lite exists to demonstrate how a system can:

- coordinate multiple services without centralizing logic
- separate decision-making from orchestration
- provide explainable outcomes instead of opaque results
- evolve incrementally without requiring a full redesign

This is not a feature-heavy system. It is a structure-driven system.

## What it demonstrates

Dispatch Lite is designed to demonstrate:

- clear service boundaries in a multi-component system
- orchestration as a separate responsibility from business logic
- assignment and prioritization as an explicit decision layer
- explainability as a first-class system capability
- deterministic, data-driven behavior using a predefined route/ETA matrix
- controlled system evolution toward async processing and externalized services
- architectural trade-offs that favor clarity over premature complexity

## System at a glance

The system is composed of five main parts:

- **UI** — presents dispatch scenarios and results
- **Orchestrator** — coordinates requests and manages system flow
- **Assignment Engine** — evaluates and selects optimal delivery assignments
- **Advisor** — explains decisions in a human-readable way
- **Data layer (CSV-first)** — provides controlled operational input

The system intentionally avoids external map services and uses a predefined route matrix. This ensures deterministic behavior, easier testing, and full control over the domain.

## Core architecture

Dispatch Lite follows an orchestration-centered architecture with separated decision and explanation layers:

UI
↓
Orchestrator
├─→ Assignment Engine
└─→ Advisor
↑
Assignment reasoning metadata
↑
CSV-backed operational data

Key architectural idea:

- The Orchestrator coordinates but does not decide
- The Assignment Engine decides but does not explain
- The Advisor explains but does not decide

This separation is intentional and forms the core architectural principle of the system.

## Phase 0 status

Dispatch Lite is currently in Phase 0: architecture and documentation lock.

At this stage:

- system direction and boundaries are defined
- data model and contracts are being documented
- explainability approach is designed
- no implementation has started yet

This is a deliberate docs-first phase to ensure that the system is well-structured before any code is written.

## Documentation map

The initial documentation package includes:

- docs/architecture/system-overview.md — high-level architecture and flow
- docs/boundaries/service-boundaries.md — responsibilities and ownership per service
- docs/data/csv-model.md — CSV-first data model and entities
- docs/contracts/component-contracts.md — interaction boundaries between components
- docs/contracts/reasoning-metadata.md — decision metadata for explainability
- docs/advisor/advisor-v1-rule-based.md — initial advisor design
- docs/setup/local-setup.md — local development setup
- docs/roadmap/milestones.md — execution plan and checkpoints
- docs/decisions/ — architecture decision records (ADRs)

These documents define the system before implementation begins.

## Planned evolution

Dispatch Lite starts from a controlled baseline and is designed to evolve incrementally:

- introduction of queues for async processing
- extraction of the Assignment Engine into a Python-based optimization service
- expansion of the Advisor with retrieval capabilities (SOPs, policies, internal docs)
- transition from CSV to a persistent data layer when needed

The system is intentionally designed so that each of these steps can be introduced without breaking existing boundaries.

## Principles and constraints

The project follows a strict set of principles:

- docs-first development
- MVP before sophistication
- clear separation of concerns
- no unnecessary infrastructure
- everything must be explainable
- frequent, small, reviewable changes

Current constraints:

- no maps API
- no GPS realism
- predefined route / ETA matrix
- CSV-first data model
- no database yet
- no queues yet
- no Python runtime integration yet
- no retrieval layer yet
- Workers-first runtime planned later
- monorepo planned, not yet bootstrapped

These constraints are deliberate and help keep the focus on architecture, decision flow, and explainability rather than infrastructure complexity.
