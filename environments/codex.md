# Codex

Little has been checked here. The first trial of `guide.md` is Codex on
Windows (2026-11-09): what it finds belongs here, with sources.

## From the Codex docs (read 2026-11-09)

The pages under `developers.openai.com/codex/` redirect to
`learn.chatgpt.com/docs/`. No page shows a date; the Windows page cites
Codex CLI 0.162.0.

- **Native Windows.** The desktop app, the CLI and the IDE extension run
  natively; commands run in PowerShell under a Windows sandbox. WSL2 is
  the alternative for Linux tooling (the Windows sandbox page).
- **The Windows sandbox** (`windows.sandbox`):
  - `mxc` — "the recommended sandbox on compatible Windows devices"
    (Windows 11 24H2, build 26100.9278, or 25H2, build 26200.9278, and
    later): no administrator setup, no extra accounts, no firewall
    rules. The desktop app prefers it for consumer accounts when the
    device supports it; the CLI uses it with `[features] prefer_mxc =
    true`.
  - `elevated` — the preferred legacy fallback, and Codex's choice when
    no mode is set: its setup raises an administrator (UAC) prompt, which
    can be declined, and creates lower-privilege sandbox users, firewall
    rules and local policy changes. A change to the machine: the user
    decides it, told what it does.
  - `unelevated` — a restricted token and file permissions; network
    isolation weaker than `elevated`; no denied read paths.
  - Deny-read globs are evaluated when each process starts. Windows 10
    is "best effort".
- **Sandbox and approvals** (the agent approvals and security page).
  Sandbox modes `read-only`, `workspace-write`, `danger-full-access` (no
  sandbox, no approvals; "not recommended"); approval policies
  `on-request`, `never`, `granular`. A folder under version control
  starts in Auto (`workspace-write`, `on-request`: edits in the folder
  without asking, asks before editing outside it or using the network);
  a folder not under version control — this guide downloaded as a ZIP —
  starts `read-only`, and Codex may start `read-only` until the folder is
  trusted (an onboarding prompt or `/permissions`). Network access is off
  by default.
- **Settings.** The user's `config.toml` is in `CODEX_HOME` (`~/.codex`;
  on Windows, for example, `C:\Users\<name>\.codex`); a repository may
  carry `.codex/config.toml`; `trust_level = "untrusted"` for a project
  disables its project-local configuration.
- **Instructions.** A global `AGENTS.md` in Codex's home, then each
  `AGENTS.md` from the project root down to the folder opened; later ones
  come later in the prompt and override.

## From a research report made for this guide (2026-11-09)

Citing the Codex docs of 2026; not checked here:

- Permission profiles (Beta): read, write and deny levels; built-in
  read-only, workspace and full access; the docs say to set the default
  explicitly.
- Its hooks are "not a complete enforcement boundary" (the docs, as
  quoted there).
- No guided set-up or interview is recorded.

## A vault built for Claude Code

A vault whose enforced rules are Claude Code's hooks
(`.claude/settings.json`) is not enforced under Codex: Codex reads the
vault's `AGENTS.md` but runs none of those hooks. Git's own hooks (a
pre-commit set by `core.hooksPath`) still run on a commit. Open such a
vault with the tool it was built for.

## Not known here

Whether Codex may write its own `config.toml`, and whether such a write
is protected; the syntax of its command rules (the execution policy) and
how they match; how a repository's `.codex/config.toml` combines with
the user's, and whether it can loosen it; where its login is stored on
Windows.
