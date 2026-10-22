---
name: spec-verify
description: Check the implementation against the spec's acceptance criteria and business rules, and find missing, off-spec, or out-of-scope parts. Use after spec-implement and code-review, or when the user asks to verify a feature against its spec.
---

# spec-verify

Compare the real code with `spec.md` and report a verdict. This skill reads and runs; it changes no code and writes no file. Gaps go back to spec-implement.

## Input and output

- Input: `docs/specs/<feature-slug>/spec.md`, `design.md`, `tasks.md`, and the uncommitted changes in the repository.
- Output: a report to the user, with a **verdict**: `pass` or `fail`.

## Steps

### 1. Load

Read `spec.md`, `design.md`, and `tasks.md`, and view the diff of the uncommitted changes. If `tasks.md` still has unticked steps, tell the user and stop.

Done when: you have the list of acceptance criteria, business rules, and changed files.

### 2. Run checks

Run the full test suite and the build.

Done when: you have the pass or fail result of each.

### 3. Verify each acceptance criterion

For each AC, find the code that implements it and the evidence that it works: a specific test, or, in a project without tests, the result of exercising the change directly. Mark it `met`, `partial`, or `missing`.

Done when: every AC has a status and evidence (file:line or test name).

### 4. Verify each business rule

Read the code to find where each BR is enforced: validations, permissions, edge states, error cases.

Done when: every BR has a location in the code or is recorded as a gap.

### 5. Find extras

Check the diff for changes that trace to no step in `tasks.md`, and for departures from `design.md` that carry no `(deviation: ...)` note.

Done when: every changed file traces to a step or is listed as out of scope.

### 6. Report

Report to the user:

- **Verdict**: `pass` only when every AC is `met`, every BR is enforced, there are no unexplained extras, and tests and build pass. Otherwise `fail`.
- **AC table**: AC, status, evidence.
- **Gaps**: missing or off-spec parts, each with file paths.
- **Out of scope**: unexplained changes.
- **Next step**: spec-sync on `pass`; spec-implement with the gap list on `fail`.

Then stop.

Done when: the report covers every AC, every BR, and every changed file.
