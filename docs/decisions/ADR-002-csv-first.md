# ADR-002: CSV-first data model for the initial system

## Status
Accepted

## Context

Dispatch Lite needs a controlled operational dataset for drivers, vehicles, deliveries, ETA values, and prioritization rules. The project is still in an architecture-first phase, and early implementation should focus on decision flow and boundaries rather than storage infrastructure.

Introducing a database at this stage would add operational complexity without improving the architectural learning goals of the project.

## Decision

Dispatch Lite will start with a **CSV-first data model**.

CSV files will act as the source of truth for the MVP and early implementation phases. These files will provide the operational input used to build the normalized snapshot consumed by the Assignment Engine.

## Consequences

This decision gives us:
- deterministic and transparent input data
- easy inspection and manual editing
- low setup overhead
- reproducible scenarios for testing and explanation

This decision intentionally postpones:
- persistent storage
- historical data management
- concurrent updates
- stateful operational workflows

The trade-off is that CSV is not a long-term storage model. It is a controlled starting point that keeps the focus on system design, not infrastructure.

## Alternatives considered

- introducing a relational database from the start
- using a managed cloud data store immediately
- embedding sample data directly into code

## Follow-up notes

This decision requires:
- a clean CSV structure
- a normalization step before decision evaluation
- clear separation between source data and derived runtime data

It also preserves a future path toward persistent storage without changing the Assignment Engine boundary.