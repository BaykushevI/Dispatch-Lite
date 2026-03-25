# Milestones

## Roadmap philosophy

Dispatch Lite is being built as a **docs-first, architecture-driven project**.
Each phase must reduce uncertainty, not introduce new directions.

The roadmap is designed to:

- keep the project manageable
- force clarity before implementation
- preserve clean service boundaries
- prevent premature complexity
- create small, reviewable checkpoints

The intent is not to move fast through implementation.  
The intent is to move in the correct order.

That order is:

1. lock architecture
2. lock boundaries and contracts
3. prepare data and tooling
4. define operational flow
5. implement the MVP
6. evolve only when the current phase is stable

Each phase must produce something reviewable.  
Progress is measured by clarity and readiness, not by the amount of code written.

---

## Phase 0 - Architecture and documentation lock

### Goal

Define and lock the architectural baseline of the project before any implementation begins.

### Included work

- define the project framing in `README.md`
- define the high-level system architecture
- define service boundaries
- define the CSV-first data model
- define component contracts
- define reasoning metadata
- define Advisor V1 behavior
- define local setup expectations
- record the initial architectural decisions (ADRs)
- define the phased roadmap

### Excluded work

- repo bootstrap
- code implementation
- runtime setup
- sample data creation
- API or contract enforcement
- UI work
- service scaffolding

### Expected output

A complete first-pass documentation package that defines:

- what the system is
- how it is structured
- how components interact
- what data exists
- how explainability works
- how the project will be executed

---

## Phase 1 - Contracts and sample data

### Goal

Translate the architectural documents into concrete, implementation-facing inputs without starting service implementation.

### Included work

- define the initial sample CSV set
- clarify the structure of the normalized input snapshot
- validate that all required data for decision-making and explanation is available
- refine interaction contracts where needed
- refine reasoning metadata where needed
- define placeholder request scenarios for manual testing
- identify missing assumptions before repo bootstrap

### Excluded work

- service logic
- UI implementation
- real request handling
- queue design
- Python runtime integration
- retrieval design

### Expected output

A stable definition of:

- the sample data landscape
- the operational inputs to the Assignment Engine
- the request/response expectations needed to start implementation safely

---

## Phase 2 - Repo and tooling shell

### Goal

Create the project structure and development shell needed for implementation, without introducing business logic.

### Included work

- bootstrap the repository structure
- define monorepo layout
- create top-level folders for apps, services, packages, docs, and infra
- ensure that structure reflects architecture, not implementation shortcuts
- add workspace/tooling configuration
- add formatting and linting baseline
- add a minimal validation workflow
- prepare the request-testing workspace

### Excluded work

- Assignment Engine logic
- Advisor logic
- UI features
- async processing
- database or persistence
- Python service extraction

### Expected output

A clean project shell that:

- installs correctly
- is ready for implementation
- reflects the documented architecture
- supports small, isolated changes

---

## Phase 3 - Operational flow skeleton

### Goal

Define the end-to-end system path in a lightweight, implementation-safe form before building full logic.

### Included work

- establish the main request journey
- prepare the orchestration sequence
- ensure that no component violates its defined boundary in the flow
- define the shape of the consolidated result
- define how assignment and advisor outputs align
- define error categories in practical terms
- prepare placeholders for scenario-based requests

### Excluded work

- full decision logic
- full explanation logic
- optimization behavior
- async execution paths
- retrieval or external knowledge integration

### Expected output

A stable system skeleton that makes the primary flow clear:

- request enters through the UI path
- orchestration invokes the decision layer
- the decision layer returns structured output
- the advisor consumes reasoning metadata
- a consolidated result returns to the caller

---

## Phase 4 - MVP implementation

### Goal

Implement the first working version of Dispatch Lite within the already-defined architectural boundaries.

### Included work

- shared contracts and supporting structures
- CSV-backed data loading and normalization
- validate that behavior matches documented expectations
- Orchestrator skeleton and request coordination
- Assignment Engine MVP
- Advisor V1 rule-based explanation flow
- UI output for assignments and explanations
- manual testing scenarios
- small iterations on prioritization behavior

### Excluded work

- queues
- Python runtime integration
- retrieval-enabled advisor behavior
- persistent storage
- historical analysis
- distributed deployment patterns beyond the MVP need

### Expected output

A working MVP that demonstrates:

- clear orchestration
- assignment decision flow
- prioritization behavior
- explainable outcomes
- architectural consistency with the documentation set

---

## Review checkpoints

Each phase should end with a review checkpoint.
Across all phases, confirm that:

- no responsibility leaks across service boundaries

### Phase 0 checkpoint

Confirm that:

- architecture is documented
- boundaries are unambiguous
- contracts are understandable
- explainability has a defined baseline
- no unresolved baseline decisions remain

### Phase 1 checkpoint

Confirm that:

- the CSV input set is clear
- normalized engine input is clear
- request scenarios are understandable
- reasoning metadata is sufficient for Advisor V1

### Phase 2 checkpoint

Confirm that:

- the repo structure matches the architecture
- local setup works cleanly
- tooling is stable
- validation and formatting are in place

### Phase 3 checkpoint

Confirm that:

- the end-to-end flow is structurally clear
- request/response alignment is stable
- component interaction expectations are practical
- the system can be implemented without rethinking boundaries

### Phase 4 checkpoint

Confirm that:

- the MVP works end to end
- the Assignment Engine and Advisor remain separate
- explanations are based on metadata, not inference
- no deferred complexity has leaked into the MVP

---

## Exit criteria by phase

### Phase 0 exit criteria

Phase 0 is complete when:

- all first-pass documentation files exist
- architectural direction is locked
- service boundaries are clear
- contracts are defined at conceptual level
- local setup direction is clear
- roadmap and ADR baseline are complete

### Phase 1 exit criteria

Phase 1 is complete when:

- the CSV set is finalized for MVP
- the normalized engine input is defined clearly
- sample scenarios are defined
- no critical ambiguity remains in decision inputs or explanation inputs

### Phase 2 exit criteria

Phase 2 is complete when:

- the repo shell is created
- workspace/tooling is usable
- formatting/linting baseline exists
- request testing workspace exists
- the project is ready for implementation without structural confusion

### Phase 3 exit criteria

Phase 3 is complete when:

- the operational request path is clear
- the orchestration flow is stable
- result composition is defined
- practical error categories are established
- the MVP can be implemented without changing architecture

### Phase 4 exit criteria

Phase 4 is complete when:

- a working MVP exists
- the main components behave according to their boundaries
- explanations are generated from reasoning metadata
- the system demonstrates its intended architectural value
- future evolution can be described without redesigning the core system

---

## Anti-scope drift rules

Dispatch Lite should remain deliberately narrow until the MVP is complete.

The following rules apply:

- do not add infrastructure before a documented need exists
- do not optimize before the system is understood
- do not introduce new services before the current boundaries are proven insufficient
- do not move Python into runtime before the Assignment Engine MVP is stable
- do not introduce queues before synchronous flow is working clearly
- do not add retrieval before Advisor V1 is complete
- do not add a database before CSV-first flow becomes a real limitation
- do not redesign boundaries during implementation unless a documented contradiction appears
- do not expand the domain with extra entities or edge cases too early

When in doubt:

- prefer the smaller version
- prefer the documented version
- prefer the explainable version
- prefer the version that preserves boundaries
