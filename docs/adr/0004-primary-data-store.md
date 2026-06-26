---
status: Accepted
y-statement: >-
  In the context of a solo, portfolio project on a one-hour/day budget that needs
  to store vector data, facing the choice of a first-choice data store, we decided
  for PostgreSQL with the pgvector extension and against flat files or per-type
  specialized stores, to achieve low infrastructure cost and portfolio value from a
  valuable extension, accepting a somewhat blunt-looking architecture and the need
  to learn a new extension.
---

# ADR 0004 — Primary data store

**Date:** 2026-06-25

## Context

- Solo developer: confident in PostgreSQL and in Microsoft SQL Server, with
  substantial Entity Framework experience, but no experience with PostgreSQL's
  vector extension.
- Budget of roughly one hour of development per day.
- The project needs to store vector data.

## Decision drivers

- Portfolio value.
- Cut the time spent finding a suitable place to store data.
- Minimize infrastructure cost.
- Storage of vector data.

## Considered options

### Files

Simplest to start. But storing vector data is awkward, and it adds nothing for
the portfolio. Rejected.

### PostgreSQL for everything

Very low infrastructure cost relative to the developer's effort, lets us learn
the portfolio-valuable pgvector extension, and supports vector data. Looks
somewhat blunt for a portfolio. Risk acceptable. **Chosen.**

### Microsoft SQL Server

No advantage over PostgreSQL here: the developer is equally confident in it, and
it offers nothing extra for this project. It also carries no portfolio upside.
Rejected.

### Specialized store per data type

A strong portfolio plus, but a very large infrastructure cost. Rejected.

## Decision

PostgreSQL (with the **pgvector** extension) is adopted as the first-choice data
store for all data types.

## Consequences

- **New extension.** Requires learning pgvector.
- **Routine.** The project becomes more routine to build.
