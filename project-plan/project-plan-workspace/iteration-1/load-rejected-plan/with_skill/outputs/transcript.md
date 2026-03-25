# Transcript: /project-plan load old-api-redesign

## Step 1: Read skill instructions

Read `/Users/grahamcarlyle/code/personal/experiments/project-plan-skill/skills/project-plan/SKILL.md` to understand the load workflow.

## Step 2: Resolve file path

User provided bare name `old-api-redesign`. Per the skill instructions, a bare name resolves to `plans/old-api-redesign.md`.

## Step 3: Read the plan file

Read `plans/old-api-redesign.md`. Contents:

```yaml
---
status: rejected
---
```

```markdown
# Old API Redesign

## Overview

Redesign the REST API to use GraphQL instead.

## Reason for rejection

Team decided REST is sufficient for our use case and GraphQL would add unnecessary complexity.
```

## Step 4: Check status

The plan has status **rejected**. Per the skill instructions (Load workflow, step 2):

> "If the plan has status `rejected`, warn and confirm before proceeding."

## Step 5: Warn and confirm

Since the status is `rejected`, I must warn the user and ask for confirmation before loading the plan into plan mode. The workflow stops here until the user confirms.

## Decision: Do NOT write to plan mode yet

The plan mode file should not be written to until the user confirms they want to proceed with loading a rejected plan. The response asks the user to confirm.

## What would happen if the user confirms

If the user says yes, I would:

1. Strip the YAML front matter
2. Verify a top-level heading exists (it does: `# Old API Redesign`)
3. Prepend the metadata comment: `<!-- project-plan-source: plans/old-api-redesign.md, status: rejected -->`
4. Write the prepared content to the plan mode file
5. Display the plan content (without the metadata comment) and tell the user they can review, modify, or approve it in plan mode
