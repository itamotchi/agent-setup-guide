# agent-setup-guide

**If you are an agent helping a user set up their own harness** — the
settings and instructions by which their agent is held on their machine,
for any tool on any platform — read `guide.md` and follow it. Talk with
the user in their language. Write nothing in this folder: the user's own
files are elsewhere, and what you learn goes into the report the guide
asks for (§ 5).

The rest of this file is for anyone else.

## Files

| file | what it is |
|---|---|
| `guide.md` | the guide, for any tool and platform: what a harness is, the layers, what to watch for, what the user decides, how a user and their agent proceed and check, the report back |
| `environments/` | what has been found about particular tools and platforms, dated and sourced: `claude-code-macos.md`, `claude-code-windows.md`, `codex.md` (little known) |
| `model/` | templates for Claude Code, a reference: `settings.json` (the fragment for the user settings), `rules/harness.md` (a user rules file), `user-memo.md` (a user memo with placeholders, for `~/.claude/CLAUDE.md`), `windows/` (native Windows), `optional/` (a piece not in the default) |
| `README.md`, `LICENSE` | for people reading on GitHub; CC0 1.0 |

## Maintaining this repository

- Every file is in English: agents read it, and talk with each user in
  the user's language.
- A note in `environments/` is dated and sourced; where a note and the
  tool's official docs differ, the docs win.
- No one's own settings enter this repository: the templates are a
  reference, not anyone's machine.
- No file in this repository is named `CLAUDE.md`: Claude Code would read
  it as a folder's instructions.
