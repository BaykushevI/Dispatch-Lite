# Advisor V1 Rule-Based Design

## Purpose of the Advisor

The Advisor is the explainability layer of Dispatch Lite.

The Advisor treats reasoning metadata as authoritative and does not reinterpret decision logic.

Its role is to transform structured reasoning metadata into clear, human-readable explanations.  
It answers “why” and “what changed” questions without re-evaluating decisions.

The Advisor does not:

- make decisions
- modify outcomes
- access raw data independently

It operates strictly on the output of the Assignment Engine.

---

## Why the Advisor is a separate service

Explainability is treated as a distinct capability, not as a side-effect of decision logic.

Separating the Advisor ensures that:

- decision logic remains clean and focused
- explanations are consistent and controlled
- explanation strategies can evolve independently
- the system avoids mixing business logic with presentation logic

This separation also enables future evolution (e.g., retrieval-based explanations) without changing the Assignment Engine.

---

## Goals of V1

Advisor V1 is a rule-based system with the following goals:

- provide clear explanations for assignment decisions
- produce explanations that are directly traceable to reasoning metadata
- explain both selection and rejection of candidates
- surface the most important decision factors
- reflect prioritization and scenario impact
- remain deterministic and predictable
- avoid any form of implicit reasoning or inference

V1 is intentionally simple:

- no external knowledge
- no learning
- no dynamic interpretation beyond defined rules

---

## Questions V1 must answer

Advisor V1 must be able to answer:

- Why was a specific driver selected for a delivery?
- Why were other drivers not selected?
- Which factors influenced the decision the most?
- Did prioritization (e.g., urgency) affect the outcome?
- What changed compared to a baseline scenario (if applicable)?

If the reasoning metadata does not support a question, the Advisor should not attempt to infer an answer.

---

## Inputs to the Advisor

The Advisor receives:

- assignment results:
  - selected driver per delivery
  - candidate drivers (optional)
- reasoning metadata:
  - decision identity
  - candidate evaluation
  - constraint outcomes
  - scoring breakdown (conceptual)
  - prioritization impact
  - scenario context
- contextual request information:
  - request type
  - scenario flags

The Advisor does not:

- access CSV data
- perform additional lookups
- recompute any values

All required information must be present in the input.

The Advisor assumes that all required reasoning is already encoded in the metadata.

---

## Explanation generation approach

Advisor V1 follows a rule-based interpretation model:
No step introduces new reasoning that is not present in the metadata.

1. **Identify explanation type**
   - selection
   - rejection
   - prioritization impact
   - scenario change

2. **Extract relevant metadata**
   - top decision factors
   - constraint failures
   - relative candidate evaluation
   - prioritization signals

3. **Map metadata to explanation templates**
   - predefined templates for each explanation type
   - placeholders filled with metadata values

4. **Generate structured explanation output**
   - concise, human-readable statements
   - consistent format across responses

Key characteristics:

- no inference beyond available metadata
- no recomputation of scores or decisions
- no dependence on hidden logic

---

## Explanation style and tone

Explanations should be:

- concise
- operational (focused on decisions, not theory)
- neutral and factual
- consistent across all outputs
- easy to read without domain expertise

Examples of tone:

- “Driver D-04 was selected due to lower estimated travel time and sufficient capacity.”
- “Driver D-02 was excluded because the vehicle did not meet the required load constraints.”
- “The delivery was prioritized due to urgency, affecting its position in the assignment order.”

Explanations must:

- avoid speculation
- avoid technical jargon
- avoid exposing internal scoring details

---

## Explanation output categories

These categories are stable and aligned with reasoning metadata categories.

Advisor V1 produces explanations in structured categories:

### Selection explanation

Why the chosen driver was selected.

### Rejection explanation

Why other candidates were not selected.

### Factor explanation

Which factors had the highest influence (e.g., ETA, capacity, priority).

### Prioritization explanation

How urgency or rules affected the decision.

### Scenario explanation

What changed when a scenario flag (e.g., urgent) was applied.

Each category is derived directly from reasoning metadata.

---

## Limits of V1

Advisor V1 has explicit limitations:

- no access to external knowledge (SOPs, policies)
- no ability to explain beyond available metadata
- no comparison with historical data
- no probabilistic or confidence-based reasoning
- no fallback to heuristic guessing
- no dynamic or adaptive behavior

If required data is missing, explanations may be incomplete rather than inferred.

---

## How V1 evolves into V2 and V3

The Advisor is designed to evolve without changing its core role.

### V2 — Retrieval-enabled Advisor

- integrates curated external knowledge (e.g., SOPs, rules)
- enriches explanations with supporting context
- still relies on reasoning metadata as the primary input

### V3 — Internal knowledge mode

- incorporates internal documents and policies
- supports deeper contextual explanations
- combines decision metadata with domain-specific knowledge

In all versions:

- the Advisor does not make decisions
- the Assignment Engine remains the source of truth
- reasoning metadata remains the foundation for explanations

The evolution adds context, not control.
