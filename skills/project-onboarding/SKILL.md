---
name: project-onboarding
description: Build or refresh the project context files (architecture, dependencies, conventions, child projects) and reference them from AGENTS.md. Use before starting work in an unfamiliar codebase, when context files are missing or stale, or when the user asks to onboard a project.
---

# project-onboarding

Produce **context files**: fixed-format documents an agent reads before working in this project. The format is fixed by the templates beside this file; every run produces the same files, the same headings, in the same order. Your judgement goes into the content, never the structure.

## Outputs

All paths are relative to the repository root.

| File | Template | When |
|---|---|---|
| `docs/context/overview.md` | [`templates/overview.md`](templates/overview.md) | Always, exactly one. |
| `docs/context/projects/<child>.md` | [`templates/child.md`](templates/child.md) | One per child project. None for a single-project repo. |

`<child>` is the child's directory path relative to the root, with `/` replaced by `--` (`apps/web` → `apps--web.md`). This keeps names unique and predictable.

## Steps

### 1. Load existing state

Read `AGENTS.md` and every file under `docs/context/` if they exist.

Done when: you know which context files already exist. Existing files are **refreshed** in place: same headings, content updated, stale lines removed.

### 2. Detect child projects

A **child project** is a directory below the root with its own build manifest (`package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`, `build.gradle*`, `*.csproj`, `composer.json`, `Gemfile`, ...) that builds, tests, or deploys on its own. Find them through:

- workspace declarations: `workspaces` in `package.json`, `pnpm-workspace.yaml`, `lerna.json`, `nx.json`, `turbo.json`, `[workspace]` in `Cargo.toml`, `go.work`, `settings.gradle*` includes, `<modules>` in `pom.xml`, `.sln` files;
- git submodules (`.gitmodules`);
- any other nested manifest outside dependency and build-output folders (`node_modules`, `vendor`, `dist`, `build`, `target`, `.venv`).

Children can nest (a child with its own workspace); record each at its own path and note its parent.

Done when: you have a list of every child path, or confirmed there are none. Every manifest found is either a child or explained (e.g. a test fixture).

### 3. Analyze

For the root and for each child, gather what its template asks for. Read real evidence: manifests, lockfiles, entry points, config files, CI files, a sample of source and test files. For a large child list, analyze children in parallel when your agent supports it.

Record what the environment cannot tell an agent quickly: how parts connect, unwritten conventions, the reason behind odd choices, gotchas. A command or version that is one file-read away gets one line with the file path, not a copy.

Done when: every template section for every file has evidence-backed content or `N/A`.

### 4. Write the context files

Fill the templates exactly:

- Keep every heading, in template order, with the same text. Add no other headings.
- Write `N/A` under a heading with nothing to say.
- Replace each `<...>` placeholder; delete the guidance comments.
- Delete files in `docs/context/projects/` for children that no longer exist.

Done when: each output file exists, matches its template's headings one-to-one, and contains no `<...>` placeholder.

### 5. Reference from AGENTS.md

Ensure the root `AGENTS.md` (create it if missing) contains exactly this block, with the child list filled in. Replace an existing block between the markers; leave the rest of the file untouched.

```markdown
<!-- project-context:start -->
## Project context

Read `docs/context/overview.md` before any task in this repository.
Before working inside a child project, also read its file:

- `apps/web` → `docs/context/projects/apps--web.md`
<!-- project-context:end -->
```

For a single-project repo, drop the second sentence and the list.

Done when: `AGENTS.md` has one marker block and every path in it resolves to an existing file.

### 6. Report

Tell the user which files were created, refreshed, or deleted, and list any section left `N/A` that likely should have content.
