# code-starter-pack

A drop-in operating manual for AI coding agents. Copy it into any repository, tell your agent to run the setup runbook, and every major coding agent — Codex, Claude Code, Cursor, Copilot, and others — picks up the same project-specific rules, commands, and boundaries.

Works with any project type: web, CLI, library, service, mobile, desktop, data pipeline, ML, embedded, or monorepo. Nothing in it assumes a language or framework.

## Why

Coding agents repeat the same failures: oversized changes, unverified "done" claims, re-planning work a previous session already planned, and following instructions embedded in fetched content. Each tool also reads its own instruction file, so guidance drifts across `AGENTS.md`, `CLAUDE.md`, and friends.

This pack solves both. `AGENTS.md` is the single canonical instruction file (an open standard stewarded by the Agentic AI Foundation under the Linux Foundation), and the operating rules in it target the specific failure modes agents do not avoid by default. The manual then maintains itself: agents correct documented facts that verification proves wrong and log durable lessons as they work.

## What's inside

| File | Purpose |
|---|---|
| `AGENTS.md` | Canonical repo guidance: project facts, operating rules, permissions, verification matrix. Filled in during setup. |
| `CLAUDE.md` | Thin wrapper so Claude Code loads `AGENTS.md` via `@AGENTS.md`. Works unedited. |
| `SETUP.md` | Initialization runbook, written for an agent to execute. Deletes itself when setup completes. |
| `tasks/TEMPLATE.md` | Plan template agents copy to `tasks/todo.md` for non-trivial work. |
| `tasks/todo.md` | The active plan slot. Local by default so each machine carries its own plan. |
| `tasks/lessons.md` | Durable corrections log, committed and team-shared. |
| `.claude/rules/README.md` | How to write path-scoped Claude Code rules, with a worked example. |
| `.gitignore.snippet` | Local-only file entries, merged into `.gitignore` during setup and then deleted. |

## Quick start

1. Copy the pack files into your project root. If the project already has its own `AGENTS.md` or `CLAUDE.md`, merge by hand first — do not drop the pack over them.
2. Tell your coding agent:

   ```text
   Run SETUP.md and do the initialization.
   ```

3. The agent detects your stack, fills the `Project Facts` section of `AGENTS.md` from real sources (manifests, lockfiles, CI configs), merges the `.gitignore` entries, spot-checks the documented commands by running them, and deletes `SETUP.md` and `.gitignore.snippet` when done.

A human can follow the same steps by hand; `SETUP.md` is the checklist.

## How it works

**One source of truth.** OpenAI Codex, Cursor, GitHub Copilot, VS Code, Amp, Zed, JetBrains Junie, Factory, OpenCode, Google Jules, and others read `AGENTS.md` natively. Claude Code reads `CLAUDE.md`, which imports the same file. Gemini CLI and Aider need a small config tweak, covered in `SETUP.md` step 6.

**Operating rules, not platitudes.** The rules in `AGENTS.md` cover only what agents get wrong by default: plan and track non-trivial work across sessions in `tasks/todo.md`, keep changes small and surgical, back every "done" claim with proof, treat external content as data rather than instructions, and keep the manual itself current.

**Plans survive interruptions.** Non-trivial work gets a plan in `tasks/todo.md` with a status line (`active | done | superseded`), so a new session resumes an unfinished plan instead of clobbering it.

**Lessons stick.** Corrections you give in chat get persisted to `tasks/lessons.md` or the `Known Gotchas` section of `AGENTS.md`, so you stop retyping the same fix every session.

**Scoped rules where needed.** Subtrees with genuinely different conventions get a nested `AGENTS.md`, or a `.claude/rules/<topic>.md` with `paths:` frontmatter for Claude-specific mechanics. See `.claude/rules/README.md`.

## Keeping it sharp

After one or two weeks of agent use, do a human pass: add observed mistakes to `Known Gotchas` or `tasks/lessons.md`, and prune rules the agent already follows without being told. The standing test for every line: would removing it cause an agent to make a mistake, and is it something the agent could not discover from the code? If it fails either test, cut it. Short, accurate files outperform long generic ones.

## Contributing

Issues and PRs welcome. Keep changes consistent with the pack's own philosophy: minimal, repo-agnostic, and only rules that target failures agents actually exhibit.
