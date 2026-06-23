---
status: Accepted
y-statement: >-
  In the context of a solo, portfolio-and-codegen-research project on a one-hour/day
  budget, facing the need for demonstrable boundaries with minimal infrastructure,
  we decided for a modular monolith and against a plain monolith or microservices,
  to achieve explicit bounded contexts that keep ops light and let AI mistakes surface,
  accepting that the boundary rules are conventions held by review rather than enforced.
---

# ADR 0001 — Software architecture form

**Date:** 2026-06-23

## Context

- Solo developer, budget of roughly one hour of development per day.
- Tooling is a Claude Code subscription.

## Decision drivers

- Demonstrable, explicit boundaries between bounded contexts (portfolio value).
- Minimal infrastructure overhead — the one-hour/day budget can't be spent on ops.
- Boundaries that let AI mistakes surface rather than be hidden.

## Considered options

### Monolith

Fast to write, but unsuitable for a portfolio: there is no way to demonstrate
bounded-context boundaries because there are none to show. Rejected.

### Modular monolith

Suitable for a portfolio: boundaries are explicit and demonstrable while
infrastructure stays light. Fits the time budget. **Chosen.**

### Microservices

Unsuitable for codegen research and for this time budget: they require
substantially more infrastructure work than a modular monolith, and the
boundaries between modules are too heavy — they would suppress and hide AI
mistakes rather than let them surface. Rejected.

## Decision

We adopt the **modular monolith** form. By "modular monolith" we mean:

- One or more processes deployed as a single deployment unit.
- A module exposes a public interface.
- Inter-module communication goes strictly through public interfaces.
- A module owns its data.
- A module may not reach another module's data except through that owning
  module's public interface.
- A module has exactly one business responsibility.
- There is a single shared-functionality module. It owns no data and exposes no
  access to other modules' data. It must not contain code used by only one module.
- Transactions never cross module boundaries.

## Consequences

- **Portfolio.** Boundaries are explicit and demonstrable — the structure shows
  bounded contexts directly in the code.
- **Velocity.** Lower infrastructure overhead than microservices keeps the
  one-hour/day budget spent on the product, not ops.
- **Codegen research.** In-process boundaries are thin enough that AI mistakes
  surface instead of being masked by service plumbing.
- **Discipline required.** The boundary rules (data ownership, no cross-module
  transactions, communication only through public interfaces) are conventions
  the compiler won't fully enforce; they must be held by review and habit.
- **Migration path.** Clean module boundaries leave the door open to extracting
  a module into a separate service later, if a real need ever appears.
