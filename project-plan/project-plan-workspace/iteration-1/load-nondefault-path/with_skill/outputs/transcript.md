# Transcript: load docs/plans/design.md

## Step 1: Read skill instructions and plan file

Read `/Users/grahamcarlyle/code/personal/experiments/project-plan-skill/skills/project-plan/SKILL.md` to understand the load workflow.

Read `/Users/grahamcarlyle/code/personal/experiments/project-plan-skill/evals/fixtures/docs/plans/design.md` to get the plan content.

The file contains:
- YAML front matter with `status: draft`
- A top-level heading: `# System Design Document`
- Plan body with Overview and Components sections

## Step 2: Follow load workflow

### Resolve file path
The user provided `docs/plans/design.md` which is a full relative path (not the default `plans/` directory). Per the skill instructions: "A full path like `plans/foo.md` or `docs/plans/foo.md` -- use directly." Used the path as-is.

### Check status
Status is `draft`. No warnings needed (warnings are only for `rejected` or `implemented`).

### Prepare plan content for plan mode
1. Stripped YAML front matter (`---\nstatus: draft\n---`).
2. Confirmed a top-level `#` heading exists: `# System Design Document`. No need to derive one from filename.
3. Prepended metadata comment for round-tripping: `<!-- project-plan-source: docs/plans/design.md, status: draft -->`. Used the full relative path `docs/plans/design.md` (not just `design.md`) so the non-default location survives round-trips.

### Write to plan mode file
Wrote the prepared content (metadata comment + heading + body) to the plan mode file.

### Display to user
Showed the plan content (without the metadata comment) and informed the user they can review, modify, or approve it in plan mode.
