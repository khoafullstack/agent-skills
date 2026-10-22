---
name: task-breakdown
description: Split a spec and design into ordered phases of small checkbox steps. Use after spec-explore, after a requirement change, or when the user asks to plan or break down a feature.
---

# task-breakdown

Turn `spec.md` and `design.md` into a **task list**: phases in build order, each a checkbox list of small steps. spec-implement works through it top to bottom and ticks each step; spec-verify traces steps back to acceptance criteria.

## Input and output

- Input: `docs/specs/<feature-slug>/spec.md` and `docs/specs/<feature-slug>/design.md`.
- Output: `docs/specs/<feature-slug>/tasks.md`, filled from [`templates/tasks.md`](templates/tasks.md).

## What a good step is

- **Small**: one sitting, a few files, reviewable at a glance.
- **Done means tested**: a step is ticked only when the code runs and its related tests pass. Write each step so that moment is obvious.
- **Traced**: ends with the acceptance criteria it serves, `(AC1, AC3)`. Setup steps with no criterion of their own (a migration, a config change) may omit it.

## Steps

### 1. Load the spec and design

Read `spec.md` and `design.md`. If either is missing or still holds a `<...>` placeholder, tell the user to run spec-explore first and stop.

Done when: you have the list of acceptance criteria and the design's components, data model changes, and API changes.

### 2. Group into phases

Group the work into phases in build order: foundations first (migrations, data model, shared interfaces), then the features built on them, then UI. Each phase leaves the code in a working state.

Done when: every design component and every data model or API change belongs to a phase, and no phase relies on a later one.

### 3. Write the steps

Break each phase into small steps, in the order they must be done.

Done when: every acceptance criterion in `spec.md` appears in at least one step, and every step is small enough that its "done" is obvious.

### 4. Confirm with the user

Show the phases and steps to the user and ask them to approve or adjust order, size, and missing or extra steps.

Done when: the user has approved the list.

### 5. Write and report

Fill the template: keep every heading in template order, replace every `<...>` placeholder, delete the guidance comments. Set **Next step** to test-design.

Tell the user the file path, the number of phases and steps, and any acceptance criterion that needed more than one step.

Done when: `tasks.md` matches the template's headings, has no `<...>` placeholder, and every acceptance criterion appears in a step.

## On a requirement change

When `tasks.md` already exists, keep ticked steps as they are, edit or add only the steps the change affects, and untick a ticked step whose work must be redone, appending `(redo: <reason>)`.
