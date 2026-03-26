# project-plan

Persist Claude Code plan-mode plans as markdown files in your project. Plans are saved with a status (draft, rejected, implemented) so you can track decisions over time, share them via git, and reload them in future conversations.

## Installation

If you've added the marketplace:

```
/plugin install project-plan@grahamcarlyle-plugins
```

Or install directly:

```
/plugin marketplace add grahamcarlyle/claude-code-plugins
/plugin install project-plan@grahamcarlyle-plugins
```

## Commands

### Save a plan

While in plan mode, save the current plan to your project:

```
/project-plan save
```

You'll be prompted to confirm the file path and status. Defaults to `plans/<plan-name>.md` with status `draft`.

### Load a plan

Load an existing plan into plan mode:

```
/project-plan load add-auth
/project-plan load plans/add-auth.md
/project-plan load docs/plans/design.md
```

You can provide a bare name (`add-auth`), a filename (`add-auth.md`), or a full relative path. Bare names and filenames resolve to the `plans/` directory.

### List plans

See all available plans with their status:

```
/project-plan list
```

Output:

```
plans/add-auth.md          [draft]        Add Authentication
plans/migrate-db.md        [implemented]  Migrate Database to Postgres
plans/new-api.md           —              New API Design
```

You can also list plans in a different directory:

```
/project-plan list docs/plans
```

## Plan file format

Plans are markdown files with optional YAML front matter for status tracking:

```yaml
---
status: draft
---

# Add Authentication

## Overview
Add JWT-based authentication to the API.
...
```

### Statuses

| Status | Meaning |
|--------|---------|
| `draft` | Work in progress — the default when saving |
| `implemented` | Plan has been carried out |
| `rejected` | Plan was considered and declined |
| *(none)* | No status tracking |

Loading a `rejected` plan will prompt for confirmation. Loading an `implemented` plan will inform you of its status.

## Directory convention

Plans default to a `plans/` directory at the project root. You can use any relative path — the skill remembers the original location so re-saving goes back to the same place.
