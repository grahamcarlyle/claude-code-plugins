---
name: project-plan
description: Manage project plans with Claude's plan mode. Use when the user wants to save a Claude plan to the project plans directory, or load an existing project plan markdown file as the basis for Claude planning mode. Trigger on "/project-plan" commands.
---

# Project Plan Skill

Synchronise Claude plan mode plans with project plan markdown files.

## Plan file format

Plan files are markdown documents with optional YAML front matter for metadata.

### Status field

Plans can optionally include a `status` field in YAML front matter:

```yaml
---
status: draft
---
```

Valid values:
- **`draft`** — Plan is being developed or under review
- **`rejected`** — Plan was considered and declined
- **`implemented`** — Plan has been carried out

The status field is entirely optional. A plan with no front matter or no status field is treated as having no status set.

## Commands

This skill supports two operations, passed as arguments:

- **save** — Save the current Claude plan to the project plans directory
- **load `<file>`** — Load a project plan markdown file as the starting point for Claude plan mode

If no argument is provided, ask the user which operation they want.

## Workflow

### Save: Copy a Claude plan to the project plans directory

1. **Locate the current plan file**
   - The plan file path is provided in the plan mode system message. Look for the plan file that Claude has been writing to during this session. It is typically at a path like `/tmp/claude-plan-*.md` or similar.
   - If you cannot find a plan file, check if the user has plan content in the recent conversation. Ask the user to confirm which plan to save.

2. **Read the plan content**
   - Read the plan file to get its full content.
   - Check for a `<!-- project-plan-source: ... -->` comment at the top of the plan content. If present, parse out the original filename and status to use as defaults in the steps below, then strip the comment from the content before saving.

3. **Determine the plans directory**
   - Look for an existing `plans/` directory in the project root.
   - If no `plans/` directory exists, create one at the project root.

4. **Choose a filename**
   - Ask the user what they want to name the plan file.
   - If this plan was loaded from a project plan file (detected via the source comment in step 2), default to the original filename.
   - Otherwise, derive from the plan title or first heading if present, converted to kebab-case with a `.md` extension (e.g., `add-authentication.md`).

5. **Set status (optional)**
   - Ask the user if they want to set a status for the plan (`draft`, `rejected`, `implemented`, or none).
   - If this plan was loaded from a project plan file with an existing status, default to that original status.
   - Otherwise, default suggestion: `draft`.
   - If the user chooses a status, add or update YAML front matter at the top of the plan content with the `status` field. If the plan already has front matter, merge the status into it.

6. **Write the plan file**
   - Write the plan content (with front matter if applicable) to `plans/<chosen-name>.md` in the project.

7. **Confirm to the user**
   - Show the path of the saved file, its status (if set), and a brief summary of the plan.

### Load: Use a project plan as the basis for Claude plan mode

1. **Identify the plan file to load**
   - If a file path was provided as an argument, use it directly.
   - Otherwise, look in the `plans/` directory and list available plan files.
   - If multiple plans exist, ask the user which one to load.

2. **Read the plan file content**
   - Read the selected markdown file.
   - If the plan has YAML front matter with a `status` field, note it. If the status is `rejected`, warn the user that this plan was previously rejected and confirm they want to proceed. If `implemented`, inform them this plan was already implemented.

3. **Present the plan and enter plan mode**
   - Strip the YAML front matter from the plan body before writing to the plan mode plan file (plan mode does not use front matter).
   - Prepend a metadata comment to the plan content written to the plan mode file so it can be recovered during save:
     ```
     <!-- project-plan-source: <original-filename>, status: <status-or-none> -->
     ```
     For example: `<!-- project-plan-source: add-auth.md, status: draft -->`
   - Display the loaded plan content to the user (without the metadata comment).
   - Write the plan content to the plan mode plan file so that Claude's plan mode can continue working with it.
   - Tell the user: "This plan has been loaded as the basis for planning. You can now review, modify, or approve it. Use plan mode to continue refining this plan."

4. **Continue in plan mode**
   - Proceed in plan mode as normal, with the loaded content as the current plan.
   - The user can modify, extend, or approve the plan from here.

## Wrap up

Provide a short confirmation message:
- For **save**: the file path where the plan was saved
- For **load**: confirm the plan was loaded and remind the user they can review/modify/approve it in plan mode
