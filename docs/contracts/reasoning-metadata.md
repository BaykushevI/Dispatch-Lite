# Reasoning Metadata

## Why reasoning metadata exists

Reasoning metadata is the structured output of the Assignment Engine that enables explainability.

Reasoning metadata is the only source of truth for explaining decisions.

The Assignment Engine produces decisions.  
The Advisor explains those decisions.

Reasoning metadata is the bridge between the two.

Without this layer:

- decisions would be opaque
- explanations would rely on guesswork or duplicated logic
- the system would lose traceability

Reasoning metadata ensures that:

- decisions are explainable without re-running logic
- explanations are based on facts, not assumptions
- decision logic and explanation logic remain separated

It is a **contractual output** of the Assignment Engine.

---

## Design goals

The reasoning metadata is designed to:

- **enable clear explanations**  
  Provide enough structured information to explain why a decision was made.

- **capture decision intent, not implementation detail**  
  Focus on outcomes and factors, not internal algorithm steps.

- **support multiple explanation scenarios**  
  Including selection reasoning, rejection reasoning, and prioritization effects.

- **remain stable across engine evolution**  
  Whether rule-based or optimized (e.g., future Python engine), the metadata should remain usable.

- **be independent of UI formatting**  
  The metadata should not contain human-readable text or presentation logic.

- **remain interpretable by multiple consumers**  
  The same metadata should support different explanation strategies.

---

## Minimum required metadata

The following fields represent the **minimum contract** required from the Assignment Engine.

Without these, the Advisor cannot produce meaningful explanations.

### Decision summary

- high-level description of the outcome (non-UI, structured)

### Decision identity

- decision identifier (unique per evaluation)
- assignment identifiers (per delivery)

### Assignment outcome

- selected driver for each delivery
- list of evaluated candidate drivers

### Candidate evaluation

- accepted candidates (feasible)
- rejected candidates (with reason category)

### Core decision factors

- top contributing factors for the decision (e.g., ETA, capacity, priority)
- relative importance or ranking of these factors

### Constraint outcomes

- which constraints were evaluated (e.g., capacity, availability)
- which constraints excluded candidates

### Prioritization impact

- whether prioritization influenced the decision
- how it affected ordering or selection

### Score breakdown (conceptual)

- comparative evaluation between candidates
- relative scores or ranking indicators (not raw algorithm details)

### Scenario context

- flags that influenced evaluation (e.g., urgent delivery)
- any deviations from baseline behavior

---

## Optional metadata

The following metadata enhances explanation quality but is not required for MVP functionality:

- confidence indicators (e.g., strong vs marginal decision)
- alternative ranking order (beyond selected candidate)
- estimated impact differences between top candidates
- simplified comparison summaries (e.g., “faster by X margin”)
- scenario comparison data (baseline vs modified scenario)

Optional metadata should:

- enrich explanations
- not change core contract expectations

---

## Metadata categories

These categories act as a conceptual schema for reasoning metadata.
To keep the structure clear, reasoning metadata is grouped into conceptual categories:

### 1. Decision identity

Identifies the evaluation and its outputs.

### 2. Assignment outcome

Defines what was selected and what alternatives existed.

### 3. Candidate analysis

Explains which candidates were considered and why some were excluded.

### 4. Scoring and ranking

Provides relative evaluation between candidates.

### 5. Constraint evaluation

Captures feasibility logic (capacity, availability, compatibility).

### 6. Prioritization impact

Shows how urgency or rules influenced the outcome.

### 7. Scenario context

Captures any external conditions affecting the decision.

These categories ensure the metadata is:

- structured
- consistent
- usable across different explanation types

---

## How the Advisor uses this metadata

The Advisor consumes reasoning metadata to generate explanations without re-evaluating decisions.

It uses the metadata to:

- explain **why a driver was selected**
  - based on top factors and relative ranking

- explain **why other candidates were rejected**
  - based on constraint failures or lower scores

- explain **prioritization effects**
  - how urgency or rules influenced ordering

- explain **scenario changes**
  - what changed when conditions differ (e.g., urgent flag)

The Advisor:

- interprets metadata
- maps it to explanation templates
- produces human-readable output

It does not:

- recompute decisions
- access raw data directly
- infer missing reasoning

If metadata is incomplete, explanation quality degrades predictably.

---

## What the Assignment Engine should not return

To maintain clean separation of concerns, the Assignment Engine must not return:

- human-readable explanation text
- UI-specific formatting
- raw algorithm traces or internal computation steps
- unnecessary low-level details (e.g., intermediate states)
- duplicated data already available in the input snapshot

The output should remain:

- structured
- concise
- focused on decision reasoning

---

## Stability expectations

Reasoning metadata is a **stable contract** between the Assignment Engine and the Advisor.

Changes to metadata should be backward-compatible whenever possible.

It must:

- remain consistent even if the decision logic changes
- be independent of the underlying algorithm (rule-based or optimized)
- support future explanation enhancements without breaking existing structure

Future extensions should:

- add new fields or categories
- avoid removing or redefining required fields

This stability ensures that:

- the Advisor can evolve independently
- the Assignment Engine can be replaced or upgraded without breaking explainability

---

## TODOs for later refinement

The following aspects are intentionally deferred:

- exact field naming and schema definitions
- concrete JSON examples
- validation rules for metadata completeness
- standardization of score representation
- detailed categorization of constraint types
- mapping between metadata fields and explanation templates
- handling of partial or degraded metadata scenarios

These will be defined once implementation begins and contracts need to be enforced more strictly.
