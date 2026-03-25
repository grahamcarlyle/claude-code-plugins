The old skill (pre-path-validation) would handle `/project-plan load /etc/passwd` as follows:

1. "If a file path was provided as an argument, use it directly" — no validation.
2. It would read `/etc/passwd` and treat its contents as a plan markdown file.
3. No YAML front matter exists (no status).
4. No top-level `#` heading exists, so it would derive one from the filename: `# Passwd`.
5. It would prepend `<!-- project-plan-source: passwd, status: none -->`.
6. It would display the file contents to the user and write them to the plan mode file.

This demonstrates a **path traversal vulnerability** — the old skill has no checks preventing reads of arbitrary files outside the project directory.
