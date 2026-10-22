# agent-skills

Agent skills for a spec-driven development workflow. Each skill is a `SKILL.md` in `skills/<name>/` and works with Claude Code, Codex, Pi, and Droid.

## Workflow

```
project-onboarding → spec-explore → task-breakdown → spec-implement → spec-verify → archive-task
```

Each skill writes an artifact the next one reads. A feature lives in `docs/specs/<feature-slug>/`:

| File | Written by |
|---|---|
| `spec.md`, `design.md` | spec-explore |
| `tasks.md` | task-breakdown (ticked by spec-implement) |

Finished features move to `docs/specs/archive/<YYYY-MM-DD>-<feature-slug>/`.

## Skills

| Skill | What it does |
|---|---|
| project-onboarding | Builds the project context files (architecture, dependencies, conventions) and references them from `AGENTS.md`. |
| spec-explore | Clarifies requirements with the user, maps them onto the codebase, weighs options, and writes the spec and design. |
| task-breakdown | Splits spec and design into ordered phases of small checkbox steps. |
| spec-implement | Implements the task list step by step, keeping changes in scope. |
| spec-verify | Checks the code against the acceptance criteria and business rules; returns `pass` or `fail`. |
| archive-task | Moves a finished feature's spec folder into the archive. (draft) |
| git-commit | Writes Conventional Commits messages and commits after you confirm. |

Planned skills (test-design, code-review, spec-sync, spec-change, bug-investigation, refactor-planning, decision-record) are tracked in [`AGENTS.md`](AGENTS.md).

## Install

Copy the skill folders you want into your agent's skills directory, for example `~/.claude/skills/` for Claude Code, then ask for the task by name ("onboard this project", "explore this feature").
