# Component Contracts

## Contract design principles

Component contracts in Dispatch Lite define **how services interact**, not how they are implemented.

The goal of these contracts is to:

- clearly define inputs and outputs across boundaries
- keep services loosely coupled
- isolate decision logic from orchestration
- enable explainability through structured metadata
- remain stable as the system evolves

Key principles:

- **boundary clarity over completeness**  
  Contracts define what is exchanged, not every internal detail.

- **contracts define system boundaries, not transport details**  
  The same contracts should remain valid regardless of how services are deployed or invoked.

- **structured, not raw data**  
  Components exchange normalized, meaningful structures.

- **no implicit dependencies**  
  Each component receives everything it needs explicitly.

- **decision data is separate from explanation data**  
  The Assignment Engine returns decision results and metadata, not human-readable output.

- **contracts are stable even if implementation changes**  
  Internal logic can evolve without breaking component interfaces.

---

## Main interaction points

There are three primary interaction points in the system:

1. **UI → Orchestrator**
2. **Orchestrator → Assignment Engine**
3. **Orchestrator → Advisor**

These interactions form a linear and controlled flow:

- the UI initiates a request
- the Orchestrator coordinates execution
- the Assignment Engine produces decisions
- the Advisor produces explanations

The Orchestrator acts as the single coordination point and enforces this interaction pattern.

No component communicates outside this flow in the MVP.

---

## UI to Orchestrator

### Purpose

To initiate a dispatch evaluation or scenario request and retrieve a complete, explainable result.

To translate decision data into explanations without modifying or reinterpreting the decision itself.

---

### Expected request shape

The request represents a **dispatch evaluation intent**, not raw data input.
The request does not assume knowledge of system internals.

It includes:

- request type (e.g., standard evaluation, scenario evaluation)
- optional scenario flags (e.g., urgency)
- optional context parameters (e.g., subset of deliveries or drivers)

The request does **not** include:

- raw CSV data
- precomputed assignments
- decision logic inputs

The Orchestrator is responsible for assembling the full operational context.

---

### Expected response shape

The response is a **consolidated result**, combining:

- assignment outcomes:
  - selected driver for each delivery
  - alternative candidates (optional)
- explanation outputs:
  - why decisions were made
  - what factors influenced the outcome
- contextual metadata:
  - request type
  - scenario indicators

The response is structured for UI consumption and presentation.

---

### Error categories

- invalid request format
- unsupported scenario type
- missing or unavailable operational data
- downstream service failure (assignment or advisor)

Errors are categorized, not deeply detailed at this stage.

---

## Orchestrator to Assignment Engine

### Purpose

To request evaluation of delivery assignments based on a normalized operational snapshot.

---

### Expected request shape

The request represents a **complete evaluation input**, including:

- active deliveries
- available drivers
- associated vehicle capabilities
- route / ETA matrix data
- driver availability
- prioritization rules
- optional scenario flags

The request is:

- normalized
- self-contained
- independent of raw CSV structure

The Assignment Engine does not perform additional data loading.

---

### Expected response shape

The response contains **decision output and reasoning metadata**:

- assignment results:
  - selected assignments
  - optional alternative candidates
- reasoning metadata:
  - scoring breakdown
  - constraint evaluations
  - prioritization impact
  - decision context

The response is structured for further processing, not for direct presentation.
The response is deterministic for a given input snapshot.

---

### Error categories

- invalid or incomplete input snapshot
- no feasible assignment available
- evaluation failure (e.g., inconsistent data)
- internal decision processing failure

---

## Orchestrator to Advisor

### Purpose

To generate human-readable explanations based on assignment decisions and reasoning metadata.

---

### Expected request shape

The request includes:

- assignment results from the Assignment Engine
- full reasoning metadata
- contextual request information (e.g., scenario flags)

The Advisor does not require:

- raw CSV data
- independent data lookups in the MVP

All required information is provided by the Orchestrator.

---

### Expected response shape

The response contains **structured explanation output**, including:

- explanation per assignment:
  - why a driver was selected
  - why alternatives were rejected
- prioritization explanations:
  - impact of urgency or rules
- scenario explanations:
  - what changed compared to baseline

The output is structured but human-readable.

---

### Error categories

- missing or incomplete reasoning metadata
- unsupported explanation scenario
- explanation generation failure

---

## Consolidated response back to UI

The Orchestrator combines outputs from the Assignment Engine and Advisor into a single response.

The UI is intentionally unaware of internal service boundaries.

This response includes:

- assignment results
- explanation data
- contextual metadata (e.g., request type, scenario flags)

The UI receives a complete, presentation-ready result without needing to call multiple services.

The Orchestrator is responsible for:

- aligning decision data with explanation data
- ensuring consistency across outputs
- hiding internal service boundaries from the UI

---

## Contract stability goals

The contracts are designed to remain stable across system evolution.

The normalized snapshot and reasoning metadata are the most critical stability points.

Stability is achieved by:

- isolating the Assignment Engine from raw data formats
- standardizing the normalized snapshot input
- treating reasoning metadata as a formal output of the decision layer
- keeping explanation generation separate from decision logic

Future changes should:

- extend contracts, not replace them
- preserve existing input/output expectations
- avoid breaking upstream or downstream components

---

## TODOs for later detail

The following areas are intentionally deferred:

- detailed field-level schemas for requests and responses
- explicit JSON examples
- validation rules and schema enforcement
- error payload structure and codes
- contract versioning strategy
- transport-level details (e.g., HTTP vs internal invocation)

These will be defined once implementation begins and contracts need to be enforced more strictly.
