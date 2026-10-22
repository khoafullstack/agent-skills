---
name: spec-implement
description: Implement a feature step by step from its task list and spec, ticking each step and keeping changes within scope. Use after task-breakdown or test-design, or when the user asks to implement a spec or continue implementation.
---

# spec-implement

Work through `tasks.md` from Phase 1 to the last phase, one step at a time, and tick each step as it is done. The code follows `spec.md` and `design.md`; nothing outside the current step changes.

## Input and output

- Input: `docs/specs/<feature-slug>/spec.md`, `design.md`, `tasks.md`, and the project context files under `docs/context/`.
- Output: code changes in the repository, and `tasks.md` with each finished step ticked `[x]`.

## Scope

Each step touches only the files it needs. Leave neighbouring code, names, and formatting as they are; add dependencies only where `design.md` names them. When you spot something worth fixing outside the step, note it for the final report.

The user commits. Leave every change uncommitted.

## Testing

Follow the project's own practice. If the project has tests for the area you change (check the context files and the existing test folders), write or update tests for each step's acceptance criteria and run them. If it has none, verify each step by running the build and, where possible, exercising the change directly.

## Steps

### 1. Load

Read the context files, `spec.md`, `design.md`, and `tasks.md`. Find the first unticked step; earlier ticked steps are done, so resume from there.

Done when: you know the next step and whether the project uses tests.

### 2. Baseline

Run the existing tests and build before changing anything. If something already fails, ask the user whether to proceed.

Done when: you know the starting state of tests and build.

### 3. Implement each step, in order

For each unticked step, Phase 1 through the last phase:

1. Write the code, following the interfaces in `design.md` and the conventions in the context files.
2. Verify it as described under **Testing**.
3. Tick the step `[x]` in `tasks.md`.

Stop and ask the user when a step fails verification and you cannot fix it within the step, or when `spec.md` or `design.md` is wrong, missing something, or contradicts the real code. Continue only after the user answers. If the answer changes the spec, API, or schema, suggest running spec-change instead of patching around it.

When the user approves an approach that differs from `design.md`, append `(deviation: <what and why>)` to that step's line in `tasks.md`.

Done when: the step is verified, nothing that passed at baseline now fails, and the step is ticked.

### 4. Final check

Run the full test suite and the build.

Done when: everything that passed at baseline still passes and every step in `tasks.md` is ticked.

### 5. Report

Tell the user the steps completed, any deviations, the out-of-scope notes, and the next step: code-review. Remind them the changes are uncommitted.

Done when: the report covers every deviation and out-of-scope note.
