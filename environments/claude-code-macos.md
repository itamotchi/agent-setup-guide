# Claude Code on macOS

The environment this guide was built on (2026-11-01 to 2026-11-09,
Claude Code v2.1.2xx–v2.1.31x, the desktop app's Code tab). Found in use
and in the Claude Code docs (settings, permissions, sandboxing); the
detailed notes — each setting, what it protects and costs, what was seen
in use — are still to be moved here. Templates: `model/`
(`settings.json`, `rules/harness.md`, `user-memo.md`, `optional/`).

The points most likely to matter to another user, in short:

- **The sandbox** (macOS Seatbelt) bounds each Bash command: writes in
  the session's folders and temp only, no host unless named and
  approved. Commands that need a credential the sandbox hides fail inside
  it: git over HTTPS whose credential comes through gh, and gh itself —
  hence `excludedCommands` for `git push/pull/fetch` and `gh`, which then
  run unbounded and only as a call of their own.
- **The write block** (`blockWritesOutsideWorkingDirectories`,
  v2.1.314+) refuses the file tools outside the session's folders and
  refuses every Claude Code settings file to the agent everywhere — the
  user applies settings changes by hand.
- **The read block** (`blockReadsOutsideWorkingDirectories`, v2.1.257+):
  a `true` anywhere wins; an outside `cat` or `ls` by Bash prompts even in
  Auto; Claude Code's own files under `~/.claude` are left to the
  ordinary outside-read flow.
- **Deny rules** stop the forms Claude usually writes, not a command in a
  string, another program for the same act, or a subprocess.
- **Ask rules** hold in every mode: `Edit(~/.claude/**)` for the agent's
  own files, `mcp__ccd_session__start_session` for nested sessions.
