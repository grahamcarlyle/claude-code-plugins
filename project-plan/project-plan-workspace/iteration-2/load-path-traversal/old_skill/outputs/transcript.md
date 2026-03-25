**Task**: `/project-plan load ../../etc/passwd`

1. The old skill says "If a file path was provided as an argument, use it directly" — no validation.
2. Attempted to read `../../etc/passwd` relative to the project root, resolving to `/Users/grahamcarlyle/code/personal/claude-code-plugins/project-plan/etc/passwd`.
3. That file does not exist, so the load failed with a file-not-found error.
4. **Key finding:** The old skill has NO path validation. It blindly attempts to read whatever path is provided. The attack only failed because the resolved path didn't exist — not because of any security check. A more carefully crafted traversal could reach system files.
