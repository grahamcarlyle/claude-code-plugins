---
status: implemented
---

# Improve project-plan skill

## Context

The project-plan skill synchronises Claude plan mode plans with markdown files in `plans/`. It works but has several rough edges: incorrect plan file path guidance, a friction-heavy save flow, no list command, no overwrite protection, narrow triggering, and inflexible file path handling on load. We want to tighten all of these up and then run test cases to verify it handles real-world variation.

## Changes

Rewrite `skills/project-plan/SKILL.md` (~108 lines → ~85 lines) with these improvements:

### 1. Fix plan file discovery (bug)
- Replace incorrect `/tmp/claude-plan-*.md` with `~/.claude/plans/` — the actual location Claude Code uses
- Reference "the path from the plan mode system message" so it stays robust

### 2. Reduce save friction
- Merge the two separate questions (filename, then status) into a **single confirmation step** with defaults
- When re-saving a previously loaded plan, defaults come from the metadata comment — user just confirms
- Example prompt: "Save as `plans/add-auth.md` with status `draft`? (Confirm or specify changes)"

### 3. Add "list" command
- Scan `plans/` for `.md` files, show filename, status, and first heading
- If no plans exist, suggest saving one

### 4. Handle file overwrites
- Before writing, check if target file exists
- If it does and this isn't a re-save of the same file, warn and confirm

### 5. Improve triggering
- Expand description to trigger on natural language: "save this plan", "load the auth plan", "what plans do we have"

### 6. Flexible file paths (load and save)
- Default directory is `plans/` but honour explicit paths the user provides
- On load: accept `plans/foo.md`, `foo.md`, bare `foo`, or a full path like `docs/plans/foo.md`
- On save: if the source comment contains a path outside `plans/`, default to that path; otherwise default to `plans/`
- On list: scan `plans/` by default, accept an optional path argument to scan elsewhere
- When resolving ambiguous names (bare `foo`), look in `plans/` first

### Preserved mechanisms
- `<!-- project-plan-source: path, status: X -->` metadata comment for round-tripping (store relative path, not just filename, so non-default locations survive round-trips)
- Heading-from-filename derivation for status line naming
- YAML front matter with `status` field
- Strip front matter on load; strip metadata comment on save

## File to modify

- `skills/project-plan/SKILL.md` — full rewrite

## Test cases

After implementing, run these test prompts via skill-creator eval to verify real-world variation:

1. **Save a new plan** — User has an active plan, says `/project-plan save`, no prior load. Should propose a filename from the heading, status `draft`, create `plans/` if needed.
2. **Load and re-save round-trip** — Load `plans/add-auth.md` (with status `draft`), approve, make edits, then save. Should default to original filename and status.
3. **Load with bare name** — `/project-plan load add-auth` (no path prefix, no `.md`). Should resolve to `plans/add-auth.md`.
4. **List plans** — `/project-plan list` with 2-3 plans on disk (mixed statuses). Should show all with status and title.
5. **Load a rejected plan** — Load a plan with `status: rejected`. Should warn before proceeding.
6. **Save with overwrite** — Save to a filename that already exists. Should warn and confirm.
7. **Non-default path** — `/project-plan load docs/plans/design.md`. Should load from the explicit path, and re-saving should default back to `docs/plans/design.md`.