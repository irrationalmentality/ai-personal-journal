---
status: Accepted
y-statement: >-
  In the context of a solo, portfolio-and-codegen-research project on a one-hour/day
  budget, facing a choice of implementation stack, we decided for C# .NET and against
  Python or a PostgreSQL/SQL-centric build, to achieve portfolio value and high
  personal velocity, accepting a less platform-free stack and a smaller surface for
  AI codegen research.
---

# ADR 0002 — Implementation stack

**Date:** 2026-06-24

## Context

- Solo developer: strong in C#/.NET and PostgreSQL, moderate in Python.
- Budget of roughly one hour of development per day.

## Decision drivers

- Portfolio value.
- Research into AI code generation.

## Considered options

### C# .NET

High personal velocity. The platform is not maximally "free", and the effort may
date. Risk acceptable. **Chosen.**

### Python

Fully free platform, and the lack of tooling for enforcing module boundaries
gives a larger surface for AI mistakes to surface — good for codegen research.
But the low barrier to entry and a saturated market make it weak for the primary
portfolio goal, and personal velocity is lower than in .NET. Rejected.

### PostgreSQL / SQL-centric

Would substantially deepen PostgreSQL knowledge, but misses the primary goal
entirely. Rejected.

## Decision

Development will be done in **C# .NET**.

## Consequences

- **Portfolio.** Demonstrates the developer's strongest skills.
- **Routine.** The project becomes more routine to build.
- **Codegen research.** Not the maximum possible surface for AI codegen research.
