# System Overview

## Purpose of the system

Dispatch Lite models a controlled delivery dispatch domain with a focus on architectural clarity rather than real-world complexity.

The system is designed to evaluate delivery assignments across available drivers, apply prioritization rules, and produce outcomes that can be clearly explained. It separates coordination, decision-making, and explanation into distinct components to demonstrate how these concerns interact in a structured system.

The purpose is not to simulate logistics at scale, but to show how a system can be designed so that:

- decisions are explicit and traceable
- responsibilities are clearly separated
- behavior is deterministic and testable
- evolution can happen without structural redesign

---

## Scope of the MVP

The MVP focuses on a **synchronous, controlled dispatch flow** using predefined data.

Included in scope:

- delivery-to-driver assignment evaluation
- prioritization based on predefined rules
- deterministic ETA calculations via route matrix
- explainable decision output
- clearly separated service responsibilities

Not included in scope:

- real-time GPS tracking
- dynamic routing or map integration
- persistent storage or historical tracking
- asynchronous processing (queues, background jobs)
- external optimization services
- retrieval or knowledge-based reasoning

The MVP is intentionally narrow to ensure architectural clarity before adding complexity.

---

## High-level architecture

Dispatch Lite follows an orchestration-centered architecture with explicit separation between coordination, decision-making, and explanation.

UI
↓
Orchestrator
├─→ Assignment Engine
└─→ Advisor
↑
Assignment reasoning metadata
↑
CSV-backed operational data

The system operates around a single entry point (Orchestrator), which coordinates interactions between components but does not contain business decision logic.

The Assignment Engine evaluates possible assignments and returns structured decision data.

The Advisor consumes this decision data and produces human-readable explanations.

The data layer provides controlled, predefined input via CSV files and is not treated as a dynamic or authoritative system of record.

---

## Main components

UI

The UI presents dispatch scenarios and outputs.
It does not perform decision-making or data processing.

Its role is limited to:

- sending requests
- displaying assignment results
- displaying explanations

⸻

Orchestrator

The Orchestrator is the central coordination layer.

It is responsible for:

- receiving incoming requests
- preparing operational context
- invoking downstream services
- combining results into a unified response

It does not:

- perform assignment logic
- evaluate prioritization rules
- generate explanations

---

Assignment Engine

The Assignment Engine is the decision-making component.

It is responsible for:

- evaluating possible delivery assignments
- applying constraints (capacity, availability, feasibility)
- applying prioritization logic
- selecting the best assignment outcome
- producing structured reasoning metadata

It does not:

- manage request flow
- format explanations
- interact with UI concerns

---

Advisor

The Advisor is the explainability component.

It is responsible for:

- interpreting reasoning metadata
- generating human-readable explanations
- answering “why” and “what changed” questions

It does not:

- influence assignment decisions
- perform optimization
- act as a knowledge retrieval system in the MVP

---

Data layer (CSV-first)

The data layer provides the operational input for the system.

It is responsible for:

- supplying drivers, deliveries, vehicles, and route data
- providing a deterministic ETA matrix
- enabling controlled, reproducible scenarios

It does not:

- act as a dynamic system of record
- enforce business logic
- track historical changes

---

## Primary request flow

The primary interaction in Dispatch Lite is a synchronous dispatch evaluation request.

Conceptually, the flow is: 1. A request is initiated from the UI (e.g., evaluate assignments for current deliveries) 2. The Orchestrator receives the request and prepares the operational context 3. The Orchestrator invokes the Assignment Engine with normalized input data 4. The Assignment Engine evaluates options and returns:

- selected assignments
- alternative candidates
- structured reasoning metadata 5. The Orchestrator invokes the Advisor with the decision output 6. The Advisor generates human-readable explanations 7. The Orchestrator returns a consolidated response to the UI

This flow is intentionally linear and synchronous in the MVP to keep behavior predictable and easy to reason about.

---

Sync vs async design

In Phase 0 and the MVP, the system is fully synchronous.

Synchronous paths:

- dispatch evaluation
- assignment computation
- explanation generation
- scenario simulation (e.g., urgent delivery impact)

This ensures:

- deterministic execution
- easier debugging
- simpler system behavior
- no need for background coordination

Asynchronous processing is deliberately deferred.

Planned future async use cases:

- batch re-evaluation of assignments
- background optimization cycles
- event tracking and audit trails
- data enrichment for advisor capabilities

The architecture is structured so that async processing can be introduced later without changing core service responsibilities.

---

Orchestration vs decision-making

A central design principle of Dispatch Lite is the separation between orchestration and decision-making.
This separation also enables independent testing and evolution of the decision layer without impacting the orchestration flow.

The Orchestrator:

- controls the flow of requests
- coordinates between components
- does not make business decisions

The Assignment Engine:

- evaluates options
- applies constraints and prioritization
- produces the actual decision

This separation ensures:

- decision logic can evolve independently
- orchestration remains simple and stable
- responsibilities are clearly defined
- system behavior is easier to explain

The Advisor introduces a third layer:

- it does not make decisions
- it explains decisions based on structured metadata

Together, these three roles form a clear system boundary model:
coordinate → decide → explain

---

Evolution path

Dispatch Lite is designed to evolve incrementally without breaking its core structure.

The initial state:

- CSV-backed data
- synchronous orchestration
- rule-based assignment and explanation

Planned evolution includes: 1. Introduction of asynchronous processing

- queues for background assignment recalculation
- decoupling of request and processing paths 2. Externalization of the Assignment Engine
- moving decision logic to a dedicated Python-based service
- enabling more advanced optimization approaches 3. Advisor enhancement
- introduction of retrieval capabilities
- integration of SOPs, policies, and internal documentation 4. Data layer evolution
- transition from CSV to a persistent storage solution when needed

The system is intentionally designed so that these changes can be introduced without redefining component boundaries.

---

Out of scope for Phase 0

The following are explicitly out of scope at this stage:

- real-time location tracking or GPS integration
- external map APIs or dynamic routing
- database-backed persistence
- asynchronous processing or queues
- Python runtime integration for optimization
- retrieval or knowledge-based reasoning
- complex event-driven architectures
- production-level observability or monitoring

These exclusions are deliberate and ensure that Phase 0 remains focused on architectural clarity, not infrastructure complexity.
