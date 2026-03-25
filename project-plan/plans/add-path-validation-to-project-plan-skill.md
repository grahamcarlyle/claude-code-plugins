---
status: draft
---

# Add Path Validation to project-plan Skill

## Context

A security review identified that the SKILL.md instructions don't enforce path boundaries. The load, save, and list workflows all accept user-provided paths without requiring validation against path traversal (e.g., `../../etc/passwd`). While the practical risk is low (the user already has local access), this should be hardened before sharing the plugin.

## Change

**File:** `project-plan/skills/project-plan/SKILL.md`

Add a **Path safety** section after the "Plan file format" section and before "Commands", containing these rules:

1. All file paths must be resolved relative to the project root
2. After resolution, the normalized absolute path must remain within the project root — reject any path that escapes it (e.g., containing `..` that resolves outside)
3. Reject absolute paths — only relative paths are accepted
4. Apply this validation in all three workflows (save, load, list) before any file I/O

Then add a brief validation step to each workflow:
- **Save** (step 3): validate the chosen path before confirmation
- **Load** (step 1): validate the resolved path before reading
- **List** (step 1): validate the scan directory before listing

## Verification

- Read through the updated SKILL.md to confirm the path safety rules are clear and consistently referenced in all three workflows
- Check the eval fixtures still make sense with the new constraints (they use `plans/` relative paths, so they should be fine)