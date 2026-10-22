# AGENTS.md

This repo builds a set of agent skills for a spec-driven development workflow. Each skill lives in `skills/<name>/SKILL.md`.

## Skills to build

Build P0 first, then P1, then P2. Within each priority, rows follow workflow order. Tick the box when a skill is done.

### P0: core workflow

| Done | # | Skill | Purpose |
|---|---|---|---|
| [x] | 1 | project-onboarding | Analyze the codebase, architecture, dependencies, and conventions before any work. |
| [x] | 2 | spec-explore | Clarify requirements with the user, map them to the codebase, weigh options by impact, and write the spec and technical design. |
| [x] | 3 | task-breakdown | Split spec and design into ordered phases of small checkbox steps, each traced to acceptance criteria. |
| [x] | 4 | spec-implement | Implement code task by task, following spec and design, minimizing out-of-scope changes. |
| [x] | 5 | spec-verify | Check implementation and tests against acceptance criteria; find missing or off-spec parts. |

### P1: quality and change

| Done | # | Skill | Purpose |
|---|---|---|---|
| [ ] | 6 | test-design | Generate test cases from the spec: happy path, edge cases, negative cases. Runs after task-breakdown, before spec-implement. |
| [ ] | 7 | code-review | Review code quality, bugs, security, performance, and architecture violations. Runs after spec-implement. |
| [ ] | 8 | spec-sync | Sync the spec with the implementation after completion to prevent spec drift. Runs after spec-verify. |
| [ ] | 14 | archive-task | Move a finished feature's spec folder to `docs/specs/archive/`. Runs after spec-sync. |
| [ ] | 9 | spec-change | Manage requirement changes, update the spec, re-assess affected tasks. Loops back to spec-explore. |
| [ ] | 10 | bug-investigation | Reproduce bugs, find root cause, propose fixes that avoid regression. |

### P2: supporting

| Done | # | Skill | Purpose |
|---|---|---|---|
| [ ] | 11 | refactor-planning | Plan controlled refactors that preserve behavior and limit breaking changes. |
| [ ] | 12 | decision-record | Record important technical decisions and trade-offs as ADRs. Callable from any design step. |
| [x] | 13 | git-commit | Write commit messages following the repo's convention (Conventional Commits by default) and commit after the user confirms. Callable any time. |

## Workflow

Main flow; each skill's output is the next one's input:

`project-onboarding → spec-explore → (decision-record) → task-breakdown → test-design → spec-implement → code-review → spec-verify → spec-sync → archive-task`

Side flows:
- `spec-change → spec-explore → task-breakdown` (re-plan affected tasks only)
- `bug-investigation → spec-implement → spec-verify`
- `refactor-planning → task-breakdown → spec-implement → spec-verify`

## Notes

- Load the `writing-for-agents` skill before creating or editing any skill.
- Frontmatter must work across Claude Code, Codex, Pi, and Droid: use only the shared fields `name` (kebab-case, same as the folder name) and `description`. Agent-specific fields go nowhere in `SKILL.md`.
- Describe actions by intent ("read the file", "search the codebase", "run the tests", "ask the user"), so each agent maps them to its own built-in tools. Built-in tool names are agent-specific and never appear in a skill.
- Each skill ends every step on a checkable completion criterion and names the artifact it produces (file path + format) so the next skill can consume it.
- A skill's `description` states what it does and the triggers for using it; keep it short.
- After finishing a skill, tick its box in the tables above.
