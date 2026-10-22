---
name: git-commit
description: Write a commit message for the current changes following the repository's commit convention (Conventional Commits by default) and commit after the user confirms. Use when the user asks to commit, write a commit message, or split changes into commits.
---

# git-commit

Turn the current changes into one or more commits whose messages follow the repository's **convention**. The user approves every message before anything is committed.

## Convention

The repository's own convention wins. Find it in this order:

1. A commit lint config (`commitlint.config.*`, `.commitlintrc*`, `commitlint` in `package.json`), a `.gitmessage` template, or commit rules in `CONTRIBUTING.md` or the context files.
2. The style of the last 20 commits in the log.
3. Otherwise, **Conventional Commits**:

```
<type>(<scope>): <subject>

<body>

<footer>
```

- **type**: `feat` (new feature), `fix` (bug fix), `refactor` (no behaviour change), `perf`, `test`, `docs`, `style` (formatting only), `build`, `ci`, `chore`, `revert`.
- **scope**: optional; the module, package, or child project changed (`auth`, `api`, `web`).
- **subject**: imperative mood, lowercase start, no trailing period, at most 72 characters for the whole first line. Says *what* changed.
- **body**: optional; wrap at 72 characters. Says *why*, and what the subject leaves out.
- **footer**: `BREAKING CHANGE: <what breaks and how to migrate>` for a breaking change (also add `!` after the type or scope: `feat(api)!:`); issue references such as `Refs: #123` or `Closes: #123`.

## Steps

### 1. Read the changes

Check the repository status and read the diff of staged changes. If nothing is staged, read the diff of all uncommitted changes, including untracked files.

Done when: you know every changed file and what changed in it.

### 2. Find the convention

Look for the sources listed under **Convention** and pick the one that applies.

Done when: you know the format, the allowed types, and how scopes are named.

### 3. Group into commits

One commit per logical change. When the changes mix unrelated work (a feature and an unrelated fix, or a refactor and a behaviour change), propose splitting them and list the files for each commit. Leave out files that should not be committed (secrets, local config, build output) and tell the user.

Done when: every changed file belongs to exactly one proposed commit or is listed as left out.

### 4. Write the messages

Write one message per commit following the convention. Pick the type from what the change does, not from the files it touches. Mark a breaking change whenever a public API, schema, config, or CLI changes in a way callers must adapt to.

Done when: each message passes the convention's rules (format, type, length) and its subject alone tells a reader what changed.

### 5. Confirm and commit

Show the proposed commits (files and message) to the user and ask them to approve or edit. Commit only after approval: stage exactly the files of each commit, then commit with the approved message. If a commit hook fails, show the output, fix the cause, and ask again.

Done when: every approved commit exists in the log and the status shows only the files the user chose to leave out.
