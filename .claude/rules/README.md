---
paths:
  - ".claude/rules/**"
---

# .claude/rules

Path-scoped Claude Code rules live here. This is a native feature since Claude Code v2.0.64, documented at code.claude.com/docs/en/memory.

Use a rule file when instructions apply to one folder or file type. Keep root `AGENTS.md` short and universal.

## How it works

- Every `.md` file in this directory is discovered recursively, subfolders included.
- A file without frontmatter loads at launch into every session, with the same priority as `.claude/CLAUDE.md`.
- A file with `paths:` frontmatter loads only when Claude reads a file matching the glob. This README uses that to stay out of normal sessions.
- Personal cross-project rules can live in `~/.claude/rules/`. Project rules load after them and take priority.

## Example rule

Create `.claude/rules/api.md`:

```md
---
paths:
  - "src/api/**/*.{ts,tsx}"
---

# API Rules

- Validate external inputs at the boundary.
- Check authorization, not only authentication.
- Use the project error response format.
- Add negative tests for permission failures.
```

## Facts worth knowing (verified June 2026)

- `paths:` accepts a YAML list of globs. Brace expansion like `src/**/*.{ts,tsx}` works.
- Path-scoped rules trigger when Claude reads a matching file, not on every tool use.
- After `/compact`, path-scoped rules drop out of context until a matching file is read again.
- To verify a path-scoped rule, read a file matching its glob, then run `/memory`. It will not show as loaded before a matching read.
- Cursor's `description`, `globs`, and `alwaysApply` fields do nothing in Claude Code.
- Some other tools have started reading this format (recent VS Code Copilot builds). Check your version's docs before relying on it.

## Other agents

Each tool has its own scoped-rule format: Cursor `.cursor/rules/*.mdc` with `globs`, GitHub Copilot `.github/instructions/*.instructions.md` with `applyTo:`, Windsurf/Devin `.devin/rules/` with `trigger:`. `AGENTS.md` itself scopes only by directory nesting. Add per-tool files only where a subtree genuinely needs different rules.
