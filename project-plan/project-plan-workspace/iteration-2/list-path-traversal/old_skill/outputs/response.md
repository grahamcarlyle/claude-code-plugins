The old skill does not have a `list` command. It only supports `save` and `load`. The `../../` path argument has no effect because the old skill's closest behavior (the load workflow listing) always looks in the `plans/` directory.

However, the old skill has NO path validation logic. For list specifically, the design inherently constrains to `plans/`, so the traversal doesn't cause leakage. But if this were a `load ../../plans/some-file.md`, the old skill WOULD be vulnerable.

Available plans:
- add-auth.md
- migrate-db.md
- new-api.md
- old-api-redesign.md
