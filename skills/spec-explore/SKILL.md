---
name: spec-explore
description: "Take a feature request to a finished spec and technical design: clarify requirements with the user, map them onto the codebase, weigh options by their impact, and design the chosen one. Use when a new feature or requirement arrives, a requirement changes, or the user asks to explore, assess, specify, or design a change."
---

# spec-explore

Turn a raw feature request into two fixed-format documents: a **spec** (what to build and how to know it is done) and a **design** (how to build it). task-breakdown, spec-implement, and spec-verify consume them.

## Outputs

| File                                  | Template                                     | When    |
| ------------------------------------- | -------------------------------------------- | ------- |
| `docs/specs/<feature-slug>/spec.md`   | [`templates/spec.md`](templates/spec.md)     | Always. |
| `docs/specs/<feature-slug>/design.md` | [`templates/design.md`](templates/design.md) | Always. |

`<feature-slug>` is a short kebab-case name for the feature (`export-invoices-csv`). On a re-run or a requirement change, refresh the existing files in place.

## Asking the user

Clarify **in the moment**: the instant you hit something ambiguous, missing, or conflicting, or a choice that belongs to the user, ask the user with your agent's question tool while the context is fresh.

- Batch questions that arise together, at most 5 per round, highest-impact first.
- Give each question concrete answer options and mark your recommended one.
- Record every answer as a row in **Decisions**. A question the user postpones goes to **Deferred** with the default you will assume.

Resolve from the code and context files whatever they can answer; ask only what is the user's to decide.

## Steps

### 1. Load context

Read `docs/context/overview.md` and the context file of each child project the request touches. If `docs/context/` is missing, suggest running project-onboarding first and continue only if the user declines. On a requirement change, also read the existing `spec.md` and `design.md`.

Done when: you know which project and child projects the feature touches.

### 2. Restate the request

Write the goal, scope (in and out), and requirements numbered R1, R2, ... Look for gaps of three kinds: _ambiguous_ (more than one reading), _missing_ (data, permissions, errors, edge states, limits), _conflicting_ (with another requirement). Ask about each.

Done when: every requirement traces to the user's words or to a Decisions row.

### 3. Map to the codebase

Search the codebase for the code, APIs, and data each requirement reads or changes. A requirement that contradicts existing behaviour is a conflict: ask.

Done when: every requirement has touchpoints with file paths, or is marked "new".

### 4. Compare options by impact

Propose 2-3 approaches. For each, assess its **impact**: code changed, API (breaking or not), database (schema, migration, existing data), other modules and child projects, tests to update. Recommend one and ask the user to pick. With only one viable approach, state why the others fail and ask the user to confirm.

Done when: the user has chosen an approach and its impact lists concrete file paths.

### 5. Specify

Write business rules (BR1, BR2, ...) and acceptance criteria (AC1, AC2, ... in Given / When / Then form), each naming the requirement it serves. Cover the happy path, edge cases, and error cases found in step 2.

Done when: every requirement has at least one acceptance criterion and every criterion is testable.

### 6. Design

Fill `design.md` for the chosen approach: architecture, interfaces, data model and migrations, API changes, data flow, error handling. Mark decisions worth an ADR under **Risks** for decision-record.

Done when: every requirement is covered by the design, and every API or schema change listed in the impact appears in it.

### 7. Assess feasibility

List blockers, risks, and parts that need a spike, then give a verdict: `feasible`, `feasible with risks`, or `not feasible`. On `not feasible`, tell the user and stop after writing the files.

Done when: the verdict is set and each risk names what it threatens.

### 8. Write and report

Fill each template: keep every heading in template order, write `N/A` under an empty one, replace every `<...>` placeholder, delete the guidance comments. Set **Next step** to task-breakdown.

Tell the user the file paths, the chosen approach, the verdict, and the next step.

Done when: each file matches its template's headings one-to-one, has no `<...>` placeholder, and every question is either in Decisions or in Deferred.
