# agent-setup-guide

**If you are an agent helping a user set up their own harness** — the
settings and instructions by which their agent is held on their machine,
for any tool on any platform — read `guide.md` and follow it. Talk with
the user in their language. Write nothing in this folder: the user's own
files are elsewhere, and what you learn goes into the report the guide
asks for (§ 5).

**If you are a vault's agent writing that vault's own procedures for its
users** — setting up a harness, changing it later, adding a git
repository inside the vault — read `vault-procedures.md`.

The rest of this file is for anyone else.

## Files

| file | what it is |
|---|---|
| `guide.md` | the guide, for any tool and platform: what a harness is, the layers, the test for a line of one's own, what to watch for, what the user decides, how a user and their agent proceed and check, the report back |
| `environments/` | what has been found about particular tools and platforms, dated and sourced, tried and read told apart: `claude-code-macos.md`, `claude-code-windows.md` (from the docs; not run on a machine), `codex.md` (the docs, trials in the ChatGPT desktop app on macOS, two reference configurations) |
| `model/` | templates for Claude Code, a reference, each line with its reason: `settings.json` (the fragment for the user settings), `rules/harness.md` (a user rules file), `user-memo.md` (a user memo with placeholders, for `~/.claude/CLAUDE.md`), `windows/` (native Windows; kept apart, no sandbox there), `optional/` (a piece not in the default) |
| `vault-procedures.md` | for a vault's agent: the base from which it writes the vault's own procedures for its users — the first setup of a user's harness, a later change, adding a git repository inside the vault (with what each tool's harness needs there) |
| `README.md`, `LICENSE` | for people reading on GitHub; CC0 1.0 |

## Maintaining this repository

- Every file is in English: agents read it, and talk with each user in
  the user's language.
- A note in `environments/` is dated and sourced, and says of each
  point whether it was tried or only read; where a note and the tool's
  official docs differ, the docs win.
- A line in a template has a reason — a problem that arose, or a silent
  harm the standard leaves (`guide.md` § 2) — or it is not there.
- No one's own settings enter this repository: the templates are a
  reference, not anyone's machine.
- No file in this repository is named `CLAUDE.md`: Claude Code would read
  it as a folder's instructions.
