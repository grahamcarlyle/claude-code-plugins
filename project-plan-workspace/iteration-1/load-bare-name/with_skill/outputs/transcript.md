# Transcript: /project-plan load add-auth

## Step 1: Read skill instructions

Read `/Users/grahamcarlyle/code/personal/experiments/project-plan-skill/skills/project-plan/SKILL.md` to understand the load workflow and other commands.

## Step 2: Resolve the file path

The user provided a bare name `add-auth`. According to the load workflow step 1, a bare name like `foo` resolves to `plans/foo.md`. So the resolved path is `plans/add-auth.md`.

Listed the `plans/` directory to confirm the file exists. Found: `add-auth.md`, `migrate-db.md`, `new-api.md`, `old-api-redesign.md`.

## Step 3: Read the plan file

Read `plans/add-auth.md`. Contents:
- YAML front matter with `status: draft`
- Top-level heading: `# Add Authentication`
- Body with overview and steps for JWT-based authentication

## Step 4: Check status

Status is `draft`. Per the load workflow step 2, warnings are only needed for `rejected` (warn and confirm) or `implemented` (inform user). No action needed for `draft`.

## Step 5: Prepare plan content for plan mode

Per load workflow step 3:
- Stripped YAML front matter (the `---` delimited block with `status: draft`)
- Confirmed a top-level `#` heading exists (`# Add Authentication`) so no need to derive one
- Prepended metadata comment: `<!-- project-plan-source: plans/add-auth.md, status: draft -->`

## Step 6: Write to plan mode file and display to user

Per load workflow step 4:
- Wrote the prepared content (metadata comment + heading + body) to the plan mode file
- Displayed the plan content to the user without the metadata comment
- Informed the user they can review, modify, or approve it in plan mode
