# ADR-005: Separate Advisor service for explainability

## Status

Accepted

## Context

Dispatch Lite is intended to demonstrate not only decision-making, but also explainability. The system needs a clear way to answer why a driver was selected, why alternatives were rejected, and how prioritization affected the result.

If explanation logic is embedded directly in the Assignment Engine, decision logic and explanation logic become tightly coupled. That would make both harder to evolve and harder to reason about.

## Decision

Dispatch Lite will use a **separate Advisor service** for explainability.

The Assignment Engine will return decision results and reasoning metadata.  
The Advisor will interpret that metadata and generate human-readable explanations.

## Consequences

This decision gives us:

- clean separation between deciding and explaining
- a first-class explainability capability
- a stable path toward future retrieval-enabled explanations
- easier evolution of explanation strategy without changing core decision logic

This decision intentionally postpones:

- embedding explanation directly in engine output
- mixed decision-and-presentation logic
- tighter but less flexible implementation shortcuts

The trade-off is one more architectural boundary in the MVP. That added boundary is justified because explainability is a core goal of the project, not an optional add-on.

## Alternatives considered

- generating explanation text directly in the Assignment Engine
- leaving explainability to the UI layer
- deferring explainability until after MVP

## Follow-up notes

This decision requires:

- stable reasoning metadata
- a clear contract between engine and advisor
- rule-based explanation logic in V1

It also creates the foundation for later retrieval and internal knowledge enrichment without changing the role of the decision layer.
