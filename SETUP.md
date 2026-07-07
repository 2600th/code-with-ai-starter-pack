# SETUP.md

Initialization runbook for this agent instruction pack. It is written for a coding agent to execute. After dropping these files into a project root, tell your agent:

```text
Run SETUP.md and do the initialization.
```

A human can follow the same steps by hand. This file deletes itself at the end of setup.

Do not drop the pack over an existing `AGENTS.md` or `CLAUDE.md`. Merge by hand first, then run setup.

## What this pack is

A drop-in operating manual for coding agents. It works with any project type: web, CLI, library, service, mobile, desktop, data pipeline, ML, embedded, or monorepo. Nothing in it assumes a language or framework. Repo specifics go into `AGENTS.md` during setup, and the manual maintains itself afterward through its own Rule 5 (Keep This Manual Current).

`AGENTS.md` is the canonical instruction file, an open standard stewarded by the Agentic AI Foundation under the Linux Foundation. OpenAI Codex, Cursor, GitHub Copilot, VS Code, Amp, Zed, JetBrains Junie, Factory, OpenCode, Google Jules, and others read it natively. Anthropic Claude Code reads `CLAUDE.md`, which imports it via `@AGENTS.md`. Gemini CLI and Aider need configuration (step 6).

## Files

```text
AGENTS.md                Canonical repo guidance. Filled during setup.
CLAUDE.md                Thin Claude Code wrapper. Works unedited.
tasks/TEMPLATE.md        Agents copy this to tasks/todo.md for non-trivial work.
tasks/todo.md            Empty plan slot. Replaced by the first non-trivial task.
tasks/lessons.md         Durable corrections log. Append rarely, prune often.
.claude/rules/README.md  Path-scoped Claude rules guidance and example.
.gitignore.snippet       Merged into .gitignore during setup, then deleted.
SETUP.md                 This runbook. Deleted at the end of setup.
```

## Initialization steps

Execute in order. This runbook is the task plan, so a separate `tasks/todo.md` is not needed for setup itself. Deleting `.gitignore.snippet` and `SETUP.md` in step 7 is pre-authorized. No other deletions are. Do not change application code anywhere during setup.

### Step 1: Preconditions

- Confirm you are at the project root: the version control root or the directory holding the top-level manifests.
- Confirm every file in the file map above exists. If any is missing, stop and report instead of improvising.
- If version history shows the repo had its own `AGENTS.md` or `CLAUDE.md` before this pack arrived, stop and ask the user how to merge. Do not discard prior instructions.
- If `AGENTS.md` contains no `__PLACEHOLDER__` markers and no `STATUS: TEMPLATE` banner, setup already ran. Stop and report.
- If `AGENTS.md` is partially filled while this file still exists, a previous setup was interrupted. Resume from the first incomplete step instead of restarting.

### Step 2: Merge .gitignore

Append `.gitignore.snippet` to the project `.gitignore` as one block preceded by a blank line, creating the file if absent. Skip non-comment lines that already exist. Preserve the file's existing encoding and line endings. Do not remove any existing entries.

### Step 3: Fill Project Facts in AGENTS.md

1. Detect the project type (web app, CLI, library, service, mobile, desktop, data pipeline, ML, embedded, monorepo). State it in the Stack section and delete rows that do not apply to this type.
2. Derive exact commands only from real sources: package manifests (package.json, pyproject.toml, Cargo.toml, go.mod, *.csproj, Gemfile, Makefile, justfile, build.gradle, CMakeLists.txt), lockfiles, CI workflows, Dockerfiles, and existing docs. Copy flags exactly. Mark slow, flaky, destructive, or external-service commands.
3. Fill Project Map only with paths an agent would misidentify or not find quickly: source-of-truth schemas, generated directories, non-obvious entry points. Delete rows the tree makes obvious. Writing down what agents can discover themselves hurts performance and costs tokens.
4. Fill Boundaries: generated and vendor directories under never-edit, config, infra, and dependency manifests under ask-first, regular source and tests under safe. List secret locations (`.env*`, key files, deploy configs).
5. In the Verification Matrix, delete rows that cannot apply here.
6. Replace every `__PLACEHOLDER__`. Mark unknowns as TODO instead of guessing. Do not invent commands you have not seen defined.
7. Keep `AGENTS.md` under 200 lines. Leave the `STATUS: TEMPLATE` banner in place for now, it comes off in step 7.

### Step 4: Mine existing knowledge into the right files

Everything added must pass two tests: would removing it cause an agent to make a mistake, and is it something an agent could NOT discover by reading the code? When unsure, leave it out.

- Examples over prose: when documenting style or workflow, prefer one short, real local example over paragraphs of explanation. Do not restate language or framework defaults.
- Known Gotchas: scan README, CONTRIBUTING, docs, CI configs, and issue or PR templates for documented quirks, required flags, ordering constraints, and footguns. Record each as one line in `AGENTS.md` Known Gotchas with a date or source reference.
- Repo etiquette: if commit message, branch, or PR conventions are documented or consistent in recent history, capture them as a `### Repo Etiquette` subsection under Project Facts. If the team wants it, include a norm that PRs declare substantial LLM-written code the author does not fully understand.
- Domain vocabulary: if the repo uses terms an outsider would misread, add a `### Domain Vocabulary` subsection (term, meaning, source of truth).
- Subtree conventions: where a subtree has genuinely different rules (a strict API layer, generated directories, docs with their own style), prefer a short nested `AGENTS.md` in that directory, which all major tools pick up. Use `.claude/rules/<topic>.md` with `paths:` frontmatter only for Claude-specific mechanics, per `.claude/rules/README.md`. Do not create either speculatively.
- Conditional workflows: keep root `AGENTS.md` for always-on rules. Move recurring multi-step workflows, rare procedures, or tool-specific behavior into scoped rules, nested `AGENTS.md` files, skills, or linked docs when the target tool supports them.
- Add no entries to `tasks/lessons.md`. Lessons are earned from observed mistakes, not generated at setup.

### Step 5: Tailor CLAUDE.md (optional)

Add to the Claude Code Notes list anything genuinely Claude-specific for this repo: plan mode triggers for its risky areas, or compaction guidance for long-running workflows. Do not duplicate anything `AGENTS.md` already says. Keep `CLAUDE.md` under 30 lines. If nothing qualifies, leave the file unedited.

### Step 6: Verify

Agent-executable checks, run them yourself:

- Run two or three of the filled commands that are safe and non-destructive (targeted test, lint, typecheck) and confirm they work exactly as written. If one fails, correct the documented entry or mark it TODO. Never change application code to make a check pass, and ask before running installs.
- Confirm no `__PLACEHOLDER__` remains in `AGENTS.md`.
- Confirm every path named in Project Map, Boundaries, gotchas, and any new nested files exists.

Hand-off checks, include these in the step 8 report for the human to run, do not attempt them yourself:

- Claude Code: run `/memory` and confirm `AGENTS.md` appears in the loaded files. The first time Claude Code sees the `@AGENTS.md` import it shows a trust dialog. If it was declined, imports stay silently disabled until the project is re-trusted in settings.
- Codex: ask `Summarize the instructions you have loaded for this session.` Codex caps concatenated `AGENTS.md` content at 32 KiB (`project_doc_max_bytes`) and silently truncates beyond it. A filled file from this pack lands around 7 to 9 KiB.
- Gemini CLI users: merge `"context": { "fileName": ["AGENTS.md", "GEMINI.md"] }` into `.gemini/settings.json`. Aider users: load with `aider --read AGENTS.md`.

### Step 7: Clean up

- Delete the `STATUS: TEMPLATE` banner from `AGENTS.md`. This is last on purpose: the banner present means setup is still in progress and commands are not yet trusted.
- Delete `.gitignore.snippet` (merged in step 2).
- Delete `SETUP.md` (this file).

### Step 8: Report

The final response must include:

- Project type detected and stack summary.
- Commands filled, and which ones were spot-checked by running them.
- Rows and matrix entries deleted as not applicable.
- Knowledge captured in step 4: gotchas, etiquette, vocabulary, and any nested `AGENTS.md` or rules files created.
- Every TODO that needs a human answer.
- The hand-off checklist from step 6.
- Files deleted (must be exactly `.gitignore.snippet` and `SETUP.md`).
- A suggested commit message. Do not commit or push unless asked.

Also tell the user how the manual stays fresh from here: `AGENTS.md` Rule 1 makes sessions resume or replace `tasks/todo.md` by its status line instead of clobbering active work, and Rule 5 makes agents persist chat corrections, fix documented facts that verification proves wrong, and delete lines that hooks or tests now enforce. Recommend a human pass after one to two weeks of agent use: add observed mistakes to `Known Gotchas` or `tasks/lessons.md` with a date or commit reference, prune rules the agent already follows without being told, and remember the standing test for every line: would removing it cause an agent to make a mistake, and could the agent discover it from the code? If it fails either test, cut it.
