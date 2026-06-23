---
name: adr
description: >-
  Use when the user wants to add, review, or reason about the project's
  Architecture Decision Records — recording a new architectural decision,
  checking existing ADRs for consistency, finding which decisions constrain a
  task, or marking one as superseded. Modes: new | check | relevant | supersede.
---

# ADR skill

Manage the project's Architecture Decision Records. The records live in
`docs/adr/`, one file per decision, named `NNNN-kebab-title.md`.

Pick the mode from the first argument: `new`, `check`, `relevant`, `supersede`.
If no mode is given, ask which one.

## The format (all modes must respect this)

Each ADR is **MADR** with a **Y-statement** in the YAML frontmatter:

```markdown
---
status: Proposed | Accepted | Superseded | Rejected
y-statement: >-
  In the context of <use case>, facing <concern>, we decided for <option>
  and against <alternatives>, to achieve <quality>, accepting that <trade-off>.
---

# ADR NNNN — <Title>

**Date:** YYYY-MM-DD

## Context
## Decision drivers
## Considered options
## Decision
## Consequences
```

This spec is the authority for the **format** — build and check records against
it; don't reverse-engineer the format from an existing file that may have
drifted. The **Accepted** records are the authority for the project's binding
rules; every mode reads them fresh.

## Mode: new <topic>

1. List `docs/adr/` and compute the next number: highest `NNNN` + 1, zero-padded
   to 4 digits. Slugify the topic for the filename.
2. Write the file from the template above. Fill Context / Decision drivers /
   Considered options (with **Chosen** / Rejected verdicts) / Decision /
   Consequences from what the user gave you. Ask for missing essentials; don't
   invent drivers.
3. **Draft the y-statement** — same compressed single-paragraph form as the
   skeleton in the format block above (context → concern → decision → rejected
   alternatives → quality → accepted trade-off), one clause each, no
   elaboration. It is a
   draft for the user to tighten, so keep it terse and factual; don't pad it.
4. Set `status: Proposed` unless the user says it's already accepted.
5. Run the `check` consistency pass on the new file before finishing.

## Mode: check

Read every file in `docs/adr/`. Report only real problems:

- Invalid or missing `status` / `y-statement` / required heading.
- Broken cross-references (a link or "Superseded by NNNN" pointing at a number
  that doesn't exist).
- A decision that contradicts a currently Accepted ADR.
- Duplicate numbers.

Output a short list grouped by file. If clean, say so in one line.

## Mode: relevant <task>

Given a task description, triage by frontmatter first — `status: Accepted` plus
the y-statement narrows the field cheaply. Then read the body of the surviving
candidates and report which ones constrain the task, with the specific clause and
the ADR number you found it in. Be concrete: name the rule and why it bites this
task, not a summary of every ADR.

## Mode: supersede <NNNN>

1. Confirm the new decision (often just-created via `new`).
2. Set the old ADR's `status: Superseded`, add `**Superseded by ADR MMMM.**`
   near its top.
3. In the new ADR add `**Supersedes ADR NNNN.**`.
4. Run `check` to confirm the back-references resolve.

## Keep it light

Solo project, ~1h/day budget. Don't add ceremony the user didn't ask for:
no status logs, no extra metadata fields, no tooling install. Numbering and
templating stay inside this skill — no external dependencies.
