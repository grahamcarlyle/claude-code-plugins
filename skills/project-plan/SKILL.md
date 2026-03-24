---
name: project-plan
description: >
  Manage project plans synced with Claude's plan mode. Trigger on "/project-plan" commands,
  and also when the user says things like "save this plan", "load the auth plan", "list plans",
  "what plans do we have", "save this plan to the project", or refers to plans in the plans/ directory.
---

# Project Plan Skill

Synchronise Claude plan mode plans with markdown files in the project. Plans default to a `plans/` directory but any path is accepted.

## Plan file format

Plan files are markdown with optional YAML front matter. The only recognised front matter field is `status`:

```yaml
---
status: draft
---
```

Valid statuses: **draft**, **rejected**, **implemented**. Omitting status entirely is fine.

## Commands

- **save** — Save the current Claude plan to the project
- **load `<file>`** — Load a plan file into Claude plan mode
- **list `[path]`** — Show available plans with their status and title

If no command is clear from context, ask the user which operation they want.

## Save workflow

1. **Find the current plan.** The plan file path appears in the plan mode system message — it will be under `~/.claude/plans/`. Read that file. If no plan file is apparent or it's empty, ask the user to confirm which plan to save.

2. **Parse metadata.** Check for a `<!-- project-plan-source: ... -->` comment at the top. If present, extract the original path and status as defaults, then strip the comment from the content.

3. **Choose defaults.**
   - *Path*: original path from metadata comment if re-saving; otherwise `plans/<heading-in-kebab-case>.md`.
   - *Status*: original status from metadata if re-saving; otherwise `draft`.

4. **Single confirmation.** Present the proposed path and status together and ask the user to confirm or adjust. Example:
   > Save as `plans/add-auth.md` with status `draft`? (Confirm, or specify changes)

5. **Check for overwrites.** If the target file already exists and this isn't a re-save of the same file, warn the user and confirm before proceeding.

6. **Ensure the target directory exists.** Create it if needed.

7. **Write the file.** Add or update YAML front matter with the status field (if a status was chosen), then write the file.

8. **Confirm** with the saved path and status.

## Load workflow

1. **Resolve the file path.** The user may provide:
   - A full path like `plans/foo.md` or `docs/plans/foo.md` — use directly
   - Just a name like `foo.md` — resolve to `plans/foo.md`
   - A bare name like `foo` — resolve to `plans/foo.md`

   If no file is specified, list available plans in `plans/` and ask the user to pick one.

2. **Read and check status.** If the plan has status `rejected`, warn and confirm before proceeding. If `implemented`, inform the user.

3. **Prepare plan content for plan mode.**
   - Strip YAML front matter (plan mode doesn't use it).
   - Ensure a top-level `#` heading exists. If missing, derive one from the filename: strip `.md`, convert kebab-case to title case (e.g., `add-auth.md` → `# Add Auth`). This heading influences the name Claude Code shows in its status line, so it matters.
   - Prepend a single metadata comment for round-tripping:
     ```
     <!-- project-plan-source: <relative-path>, status: <status-or-none> -->
     ```
     For example: `<!-- project-plan-source: plans/add-auth.md, status: draft -->`
     Store the relative path (not just filename) so non-default locations survive round-trips.

4. **Write to the plan mode file** (the path from the plan mode system message under `~/.claude/plans/`). Order: metadata comment, then heading, then plan body. Display the plan content to the user (without the metadata comment) and tell them they can now review, modify, or approve it in plan mode.

## List workflow

1. Scan the `plans/` directory (or the path provided as argument) for `.md` files.
2. For each file, read the YAML front matter status (if any) and the first markdown heading.
3. Display a list showing filename, status, and title. For example:

   ```
   plans/add-auth.md          [draft]        Add Authentication
   plans/migrate-db.md        [implemented]  Migrate Database to Postgres
   plans/new-api.md           —              New API Design
   ```

4. If no plans exist, suggest saving one with `/project-plan save`.
