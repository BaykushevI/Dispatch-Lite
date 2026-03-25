# CSV-First Data Model

## Why CSV-first is the right starting point

Dispatch Lite uses a CSV-first data model to keep the system simple, deterministic, and fully controlled in the early phases.

This approach is intentional:

- it removes infrastructure dependencies
- it allows fast iteration and easy inspection of data
- it keeps all scenarios reproducible
- it ensures the Assignment Engine receives predictable input

At this stage, the goal is not to design a scalable storage layer, but to clearly define **what data the system operates on** and **how that data is structured for decision-making**.

CSV is used as a stable, transparent source of input — not as a long-term storage strategy.

---

## Initial CSV files

The CSV files can be conceptually grouped into:

- operational entities (drivers, vehicles, deliveries)
- constraints and rules (availability, priority)
- routing data (route matrix and zones)

The system starts with a small, well-defined set of CSV files:

- **drivers.csv**  
  Contains driver identities and basic operational attributes.

- **vehicles.csv**  
  Defines vehicle capabilities and capacity constraints.

- **deliveries.csv**  
  Represents delivery tasks to be assigned.

- **routes_matrix.csv**  
  Provides predefined ETA values between zones (no maps or dynamic routing).

- **driver_availability.csv**  
  Defines when drivers are available for assignments.

- **priority_rules.csv**  
  Encodes prioritization weights and urgency behavior.

- **zones.csv** _(optional, if needed)_  
  Defines logical locations used by the route matrix.

These files together define the complete operational input for the system.

---

## Core entities

The data model is built around a small set of core entities:

### Driver

Represents an individual who can perform deliveries.

Typical attributes include:

- identifier
- assigned vehicle
- availability window
- capacity limits (directly or via vehicle)
- operational status

---

### Vehicle

Represents the transport capability used by a driver.

Typical attributes include:

- capacity constraints (volume, weight)
- type or category
- compatibility rules for deliveries

---

### Delivery

Represents a task that must be assigned.

Typical attributes include:

- origin and destination zones
- size or load requirements
- priority level
- due window or timing constraint
- status (e.g., pending)

---

### Route Matrix

Represents deterministic travel time between zones.

Typical attributes include:

- origin zone
- destination zone
- estimated time (ETA)

This replaces any need for map APIs or real-time routing.

---

### Driver Availability

Represents when a driver can accept deliveries.

Typical attributes include:

- driver reference
- available time window
- optional constraints (e.g., breaks)

---

### Priority Rules

Represents how prioritization affects assignment decisions.

Typical attributes include:

- priority level
- relative weight or rank
- urgency modifiers

---

## Entity relationships

The relationships between entities are simple and intentional:

- a **driver** is associated with a **vehicle**
- a **driver** has **availability constraints**
- a **delivery** requires a compatible **vehicle**
- a **delivery** maps to a route via the **route matrix**
- **priority rules** influence how deliveries are evaluated

These relationships are not enforced by a database.  
They are interpreted at runtime when building the input for the Assignment Engine.
All relationships are resolved at runtime and are not enforced structurally.

---

## Source of truth

For the MVP, **CSV files are the only source of truth**.

This means:

- all operational data is read from CSV
- there is no persistent state outside these files
- no system component mutates or stores authoritative data

The system operates on **snapshots of data**, not live state.

This keeps:

- behavior deterministic
- debugging simple
- scenarios reproducible

CSV files are treated as immutable inputs during execution.

---

## Derived and temporary data

During execution, the system produces data that is not part of the source CSV files.

This includes:

- feasible driver candidates per delivery
- assignment scores and rankings
- constraint evaluation results
- estimated completion times
- prioritization effects
- explanation-related facts

This data is:

- computed at runtime
- not persisted
- not part of the source of truth

It exists only to support decision-making and explanation.

---

## Normalized snapshot for the Assignment Engine

The Assignment Engine does not consume raw CSV files directly.
It represents the only contract the Assignment Engine depends on.

Instead, it operates on a **normalized operational snapshot**, which represents the current state of the system in a structured and consistent form.

This snapshot includes:

- active deliveries
- available drivers
- associated vehicle capabilities
- route matrix lookup data
- driver availability windows
- prioritization rules
- optional scenario flags (e.g., urgent delivery)

Key characteristics of this snapshot:

- it is built from CSV inputs before evaluation
- it is consistent and self-contained
- it does not require additional lookups during decision-making
- it isolates the Assignment Engine from raw data formats

This ensures that:

- the Assignment Engine remains independent of storage format
- future data source changes do not affect decision logic
- evaluation logic stays clean and focused

---

## Data assumptions and constraints

The data model operates under several explicit assumptions:

- all route times are predefined and static
- no real-time updates or live tracking exist
- data sets are small and controlled
- all required relationships can be resolved from CSV inputs
- no historical state is required
- no concurrent updates are expected
- the system operates in a controlled, non-real-time environment

These constraints are deliberate and ensure that:

- the system remains predictable
- behavior is easy to reason about
- focus stays on architecture and decision flow

---

## Future storage evolution

The CSV-first approach is a starting point, not a long-term solution.

The normalized snapshot acts as a stability layer during this transition.

As the system evolves, the data layer may transition to:

- a persistent database
- a managed storage service
- a structured event or history model

This evolution is expected to:

- preserve the normalized snapshot interface
- avoid changes to Assignment Engine inputs
- maintain separation between data access and decision logic

The goal is to allow storage to evolve independently without breaking system boundaries.
