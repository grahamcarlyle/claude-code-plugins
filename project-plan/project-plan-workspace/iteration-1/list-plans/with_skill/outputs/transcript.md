# Transcript: /project-plan list

## Step 1: Read skill instructions

Read `/Users/grahamcarlyle/code/personal/experiments/project-plan-skill/skills/project-plan/SKILL.md` to understand the list workflow.

The list workflow says:
1. Scan the `plans/` directory for `.md` files.
2. For each file, read the YAML front matter status (if any) and the first markdown heading.
3. Display a list showing filename, status, and title.
4. If no plans exist, suggest saving one.

## Step 2: Scan plans directory

Listed files in `evals/fixtures/plans/`:
- add-auth.md
- migrate-db.md
- new-api.md
- old-api-redesign.md

## Step 3: Read each plan file

### plans/add-auth.md
- Front matter status: `draft`
- First heading: `Add Authentication`

### plans/migrate-db.md
- Front matter status: `implemented`
- First heading: `Migrate Database to Postgres`

### plans/new-api.md
- Front matter status: (none)
- First heading: `New API Design`

### plans/old-api-redesign.md
- Front matter status: `rejected`
- First heading: `Old API Redesign`

## Step 4: Format and display output

Formatted the list with filename, status (or dash if none), and title, matching the example format from the skill instructions.
