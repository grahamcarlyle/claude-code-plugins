# Transcript: /project-plan load add-auth

## Step 1: Identify the plan file to load

The user provided `add-auth` as the argument (a bare name without path or extension).

**Tool call: Bash** — Listed files in `plans/` directory:
- add-auth.md
- migrate-db.md
- new-api.md
- old-api-redesign.md

Matched `add-auth` to `plans/add-auth.md`.

## Step 2: Read the plan file content

**Tool call: Read** — Read `plans/add-auth.md`.

Contents:
- YAML front matter with `status: draft`
- Top-level heading: `# Add Authentication`
- Overview and 4 implementation steps

Status is `draft` — no special warnings needed (warnings are only for `rejected` or `implemented`).

## Step 3: Present the plan and enter plan mode

Per the skill instructions:

1. **Strip YAML front matter** — Removed the `---\nstatus: draft\n---` block from the content to write to plan mode.
2. **Check for top-level heading** — The plan already has `# Add Authentication`, so no heading needs to be added.
3. **Prepend metadata comment** — Added `<!-- project-plan-source: add-auth.md, status: draft -->` as the first line of the plan mode file content.
4. **Display to user** — Showed the plan content (without the metadata comment) and the confirmation message.
5. **Write to plan mode file** — Would write the content (with metadata comment) to the plan mode plan file.

## Output

- **plan-mode-content.md**: The content that would be written to the plan mode file (metadata comment + heading + body, no front matter).
- **response.md**: What the user would see (plan content without metadata comment, plus confirmation message).
