---
status: Accepted
y-statement: >-
  In the context of a solo, one-hour/day C# project, facing how to realise the
  modular monolith's module boundaries, we decided for several assemblies per
  module with a CQRS public-interface assembly and internal-only implementation
  assemblies, and against a single assembly per module or a non-CQRS public
  assembly, to achieve clean, adaptable module contracts with boundary checks
  shifted onto the compiler and tests, accepting higher implementation complexity.
---

# ADR 0003 — Module implementation

**Date:** 2026-06-24

## Context

- Implementation language is C#: strongly typed and compiled.
- Budget of roughly one hour of development per day.

## Decision drivers

- Demonstrate skills in the portfolio.
- Minimise manual policing of boundaries.
- Research AI behaviour in code generation.

## Considered options

### One assembly per module

Public interface via public classes, internals hidden in `internal` classes.
Simple to build, but weak tool-enforced control, and the public API tends to
bloat because it has access to the implementation. Rejected.

### Public-interface assembly + implementation assemblies per module

Medium complexity; the public interface does not know the implementation. But
the public interface tends to bloat by mixing commands and data. Rejected.

### CQRS public-interface assembly + implementation assemblies

Clean public interface via CQRS. Higher implementation complexity. **Chosen.**

## Decision

Each module is **several assemblies** with an explicit public-interface assembly
following **CQRS**, plus implementation assemblies. Implementation assemblies
contain only `internal` classes, with a single exception: the DI-container
composition point. That point is not part of the module's public interface — it
is a technical compromise.

### Boundary enforcement

- **Public-interface assembly contains only DTOs** — checked by unit tests.
- **Implementation assemblies expose exactly one public point**, the DI-container
  composition point — checked by tests.
- **Modules never use each other's implementation**, only the public interface —
  enforced by the compiler.
- **The inter-module dependency graph** — checked by architecture tests
  (NetArchTest / ArchUnitNET) that assert the allowed graph.
- **Correct placement in the shared library** — checked by tests: every public
  element must be used in at least two modules.
- **Transactions must not span modules** — controlled at runtime in the mediator
  between a module's public interface and its implementation.
- **Access to data in external sources** — left to the developer, not enforced
  automatically.

## Consequences

- Clean, adaptable module contracts.
- Higher implementation complexity.
