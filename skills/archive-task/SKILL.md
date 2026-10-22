---
name: archive-task
description: Move a finished feature's spec folder into the archive so active specs list only open work. Use after spec-sync, or when the user asks to archive a done feature or task.
---

# archive-task

Move `docs/specs/<feature-slug>/` into `docs/specs/archive/`. This skill moves files only; it changes no code and no spec content beyond the archive note.

## Input and output

- Input: `docs/specs/<feature-slug>/` with `spec.md`, `design.md`, `tasks.md`.
- Output: `docs/specs/archive/<YYYY-MM-DD>-<feature-slug>/`, the same files plus an archive note at the top of `tasks.md`.

## Steps

### 1. Check it is finished

Read `tasks.md`. If any step is unticked, list those steps and ask the user whether to archive anyway; stop if they say no.

Done when: every step in `tasks.md` is ticked, or the user has approved archiving with the listed steps unticked.

### 2. Confirm the move

Show the user the source folder and the target path `docs/specs/archive/<today's date>-<feature-slug>/`. If the target already exists, report it and stop.

Done when: the user has approved the target path.

### 3. Move

Move the folder with the repository's version control (a rename, so history follows the files). Add one line at the top of `tasks.md`: `> Archived <YYYY-MM-DD>.`

Done when: the source folder no longer exists, the target holds every original file, and `tasks.md` starts with the archive note.

### 4. Fix links

Search the repository for references to the old path `docs/specs/<feature-slug>/` and update each to the archive path.

Done when: a search for the old path returns no hits outside the archive folder itself.

### 5. Report

Tell the user the new path and the files whose links changed. Suggest git-commit for the move.

Done when: the user has the new path and the list of updated files.
