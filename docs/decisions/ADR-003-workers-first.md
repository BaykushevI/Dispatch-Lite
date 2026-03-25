# ADR-003: Workers-first runtime for the initial service architecture

## Status
Accepted

## Context

Dispatch Lite is intended to model a multi-component system without introducing heavy runtime complexity in its initial phases. The system needs a practical way to express service boundaries, orchestration, and component interactions while remaining manageable for a solo project.

The runtime choice must support quick iteration, simple deployment, and a credible evolution path.

## Decision

Dispatch Lite will follow a **Workers-first runtime direction** for the initial implementation phase.

The early services will be designed with Cloudflare Workers in mind, while preserving the ability to externalize components later if the architecture requires it.

## Consequences

This decision gives us:
- a lightweight execution model
- fast iteration and simple deployability
- a practical fit for the MVP scope
- a clear way to express service boundaries early

This decision intentionally postpones:
- container-based service deployment
- Python runtime integration
- orchestration across multiple infrastructure nodes
- operational complexity beyond MVP needs

The trade-off is that not every future evolution target fits naturally inside the initial runtime. That is acceptable, because the boundaries are being designed to support later extraction.

## Alternatives considered

- starting with Docker-based local services
- starting directly with a Python optimization service
- starting with a VM or K3s deployment model

## Follow-up notes

This decision supports:
- lightweight local development
- a clean MVP implementation path
- later extraction of the Assignment Engine or Advisor if needed

It also reinforces the need for stable contracts so runtime changes do not force architectural redesign.