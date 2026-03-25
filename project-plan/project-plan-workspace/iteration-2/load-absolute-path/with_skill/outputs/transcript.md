**Task**: `/project-plan load /etc/passwd`

1. Read SKILL.md and identified the load workflow.
2. Applied path safety validation (load workflow step 2): `/etc/passwd` starts with `/`, making it an absolute path.
3. Rejected the operation per rule #1 ("Only relative paths are accepted — reject any absolute path starting with `/`").
4. No file I/O was performed. No plan-mode-content.md is produced since the load was refused.
