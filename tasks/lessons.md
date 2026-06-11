# Lessons

Durable repo-specific corrections only. Agents read this file at the start of non-trivial work, append new entries, and delete entries that verification proves false. Humans do judgment pruning.

Format: `YYYY-MM-DD | category | lesson`

Categories: `testing`, `conventions`, `commands`, `boundaries`, `performance`, `security`, `tooling`, `other`.

Add an entry when:

- The same mistake happened twice, or once with high cost.
- A review caught something the agent should have known about this repo.
- You typed the same correction you already typed in an earlier session.

Rules:

- Keep each entry under 120 characters.
- Delete entries that are no longer true.
- If a lesson needs more detail, promote it to `AGENTS.md` Known Gotchas or a path-scoped rule, then delete it here.
- This file is committed and team-shared. Agent auto-memory features are personal and machine-local, so they do not replace it.

## Entries

(none yet)
