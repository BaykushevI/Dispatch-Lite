# Service Boundaries

## Why boundaries matter in Dispatch Lite

These boundaries are intentionally strict and are treated as architectural constraints, not guidelines.
Dispatch Lite is intentionally structured around clear separation of responsibilities.  
The system is not designed as a collection of features, but as a set of components with well-defined ownership.

Clear boundaries ensure that:

- coordination logic does not absorb business decisions
- decision-making remains isolated and replaceable
- explainability is treated as a separate capability
- future evolution does not require rewriting existing components

Each service has a strict scope. Anything outside that scope is explicitly excluded.

---

## Boundary summary table

| Component         | Owns                                                     | Does not own                                          | Future note                                    |
| ----------------- | -------------------------------------------------------- | ----------------------------------------------------- | ---------------------------------------------- |
| Orchestrator      | request flow, coordination, response composition         | decision logic, scoring, explanation generation       | async coordination and queue integration       |
| Assignment Engine | assignment decisions, prioritization, reasoning metadata | orchestration, UI concerns, explanation formatting    | externalization to Python optimization service |
| Advisor           | explanation generation based on decision metadata        | decision-making, orchestration, data ownership        | retrieval-enabled explanation                  |
| Data layer        | data access, normalization, deterministic input          | business logic, decision-making, orchestration        | migration to persistent storage                |
| Retrieval layer   | knowledge lookup and enrichment (future)                 | assignment decisions, orchestration, operational data | introduced after MVP                           |

---

## Orchestrator boundary

### Responsibilities

- receives incoming requests from the UI
- prepares operational context for downstream services
- invokes the Assignment Engine
- invokes the Advisor
- aggregates and returns a consolidated response
- defines the execution flow of the system

### Non-responsibilities

- does not evaluate assignment options
- does not apply prioritization logic
- does not compute ETA or routing decisions
- does not generate explanations
- does not access knowledge or policy sources

### Inputs

- request from UI (dispatch or scenario evaluation request)
- access to normalized operational data (via data layer)
- assignment results from the Assignment Engine
- explanation output from the Advisor

### Outputs

- consolidated response containing:
  - assignment results
  - explanation data
  - contextual response information

### Why it is separate

The Orchestrator exists to isolate coordination from decision-making.  
Without this separation, business logic would become embedded in request handling, making the system harder to evolve and reason about.
It also ensures that orchestration remains stable even as decision logic evolves.
It also enables isolated testing of decision logic without involving orchestration or UI layers.
This separation ensures that explanation strategies can evolve independently of decision algorithms.

### Future evolution

- introduction of asynchronous flows (queues, background processing)
- orchestration of multiple processing paths (sync + async)
- expansion into more complex coordination scenarios without absorbing decision logic

---

## Assignment Engine boundary

### Responsibilities

- evaluates possible delivery-to-driver assignments
- applies feasibility constraints (capacity, availability, compatibility)
- applies prioritization rules
- selects the best assignment outcome
- produces structured reasoning metadata describing the decision

### Non-responsibilities

- does not coordinate request flow
- does not interact with the UI
- does not generate human-readable explanations
- does not retrieve external knowledge or policies
- does not manage system state beyond the current evaluation

### Inputs

- normalized operational snapshot:
  - deliveries
  - drivers
  - vehicles
  - route / ETA matrix
  - availability data
  - prioritization rules
- optional scenario flags (e.g., urgency)

### Outputs

- assignment results:
  - selected assignments
  - candidate alternatives
- reasoning metadata:
  - scoring breakdown
  - constraint outcomes
  - prioritization effects
  - decision context

### Why it is separate

The Assignment Engine isolates business decision logic from the rest of the system.  
This allows the decision layer to evolve independently, including replacement with more advanced optimization approaches.

### Future evolution

- replacement or extension with a Python-based optimization service
- support for more advanced decision strategies without affecting orchestration
- potential scaling as an independent service

---

## Advisor boundary

### Responsibilities

- interprets reasoning metadata from the Assignment Engine
- generates human-readable explanations
- answers “why” and “what changed” questions
- provides structured explanation output for the UI

### Non-responsibilities

- does not influence assignment decisions
- does not evaluate or optimize assignments
- does not manage request flow
- does not act as a data source for operational input

### Inputs

- assignment results and reasoning metadata from the Assignment Engine
- contextual request information (from the Orchestrator)

### Outputs

- explanation data:
  - selection explanations
  - rejection explanations
  - prioritization impact explanations
  - scenario-based explanations

### Why it is separate

Explainability is treated as a distinct capability.  
Separating it from the Assignment Engine ensures that decision logic remains clean and that explanation strategies can evolve independently.

### Future evolution

- integration with retrieval mechanisms (SOPs, policies, internal documents)
- richer explanation models based on external knowledge
- support for more advanced “what-if” and scenario reasoning

---

## Data layer boundary

### Responsibilities

- provides access to operational data from CSV sources
- normalizes raw data into a consistent structure
- supplies deterministic input to the Assignment Engine
- ensures reproducible and controlled scenarios

### Non-responsibilities

- does not apply business rules
- does not perform assignment decisions
- does not generate explanations
- does not manage orchestration or workflows
- does not act as a dynamic system of record

### Inputs

- static CSV datasets:
  - drivers
  - vehicles
  - deliveries
  - route matrix
  - availability
  - prioritization rules

### Outputs

- normalized operational snapshot for use by the Assignment Engine and Orchestrator

### Why it is separate

Separating data access from business logic ensures that storage changes do not impact decision or orchestration layers.  
It also keeps input deterministic and easier to test.

### Future evolution

- migration from CSV to persistent storage (e.g., database or managed data service)
- introduction of historical data and event tracking
- support for larger datasets without changing consumer interfaces

---

## Future retrieval layer boundary

### Responsibilities

- provides knowledge lookup capabilities (e.g., SOPs, policies, internal documents)
- enriches explanation generation with contextual information
- supports advisor-level reasoning enhancement

### Non-responsibilities

- does not perform assignment decisions
- does not coordinate system flow
- does not replace operational data sources
- does not act as a primary input to the Assignment Engine

### Why it is separate

Knowledge retrieval is fundamentally different from operational decision-making.  
Separating it prevents coupling between optimization logic and knowledge interpretation.

### Future evolution

- introduction after MVP as an extension to the Advisor
- integration with curated and internal knowledge sources
- enhancement of explanation depth without modifying core decision logic
