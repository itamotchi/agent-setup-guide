# agent-setup-guide

**If you are an agent helping a user set up their own harness** — the
settings and instructions by which their agent is held on their machine,
for any tool on any platform — read `guide.md` and follow it; where
`setup/` has a file for the user's tool and platform, that file is what
is applied, step by step. Talk with the user in their language. Write
nothing in this folder: the user's own files are elsewhere, and what you
learn goes into the report the guide asks for (§ 5).

**If you are a vault's agent writing that vault's own procedures for its
users** — setting up a harness, changing it later, adding a git
repository inside the vault — read `vault-procedures.md`: a vault's
procedure is `guide.md` § 4 and a file in `setup/` with the vault's
values put in.

The rest of this file is for anyone else.

## Files

| file | what it is |
|---|---|
| `guide.md` | the guide, for any tool and platform — what holds whatever the tool: what a harness is, the layers, the test for a line of one's own, what to watch for, what the user decides, the steps of a setup in order (the application's own settings, the files, the lines for a nested repository, the check) and of a later change, the report back |
| `setup/` | the procedure for one tool and platform: what it sets and how far it was tried, the steps in order, each value with its reason, the entries for a later change, a lighter alternative where there is one: `codex-macos.md` (Codex in the ChatGPT desktop app on macOS), `claude-code-macos.md` (Claude Code on macOS). None for Claude Code on native Windows: never run on a machine |
| `environments/` | the evidence the files in `setup/` point to: what has been found about particular tools and platforms, dated and sourced, tried and read told apart: `claude-code-macos.md`, `claude-code-windows.md` (from the docs; not run on a machine), `codex.md` (the docs, trials in the ChatGPT desktop app on macOS) |
| `model/` | the text of the templates for Claude Code: `settings.json` (the fragment for the user settings), `rules/harness.md` (a user rules file), `user-memo.md` (a user memo with placeholders, for `~/.claude/CLAUDE.md`), `windows/` (native Windows; kept apart, no sandbox there), `optional/` (a piece not in the default). The reason for each macOS line is in `setup/claude-code-macos.md` |
| `vault-procedures.md` | for a vault's agent: how a vault's own procedure is made from `guide.md` § 4 and a file in `setup/` — what is adjusted and what is not, how a user reaches it (the introductory document, the text to paste, the setup folder, the order), a later change, adding a git repository inside the vault (with what each tool's harness needs there) |
| `README.md`, `LICENSE` | for people reading on GitHub; CC0 1.0 |

## Maintaining this repository

- Every file is in English: agents read it, and talk with each user in
  the user's language.
- One fact in one place. A value and its reason are in the tool's file
  in `setup/`; the dated finding it rests on is in `environments/`;
  what is true of any tool is in `guide.md`. Text that moves is moved,
  not copied.
- A note in `environments/` is dated and sourced, and says of each
  point whether it was tried or only read; where a note and the tool's
  official docs differ, the docs win. A file in `setup/` states as
  tried only what was tried, with its conditions.
- A value in `setup/` or a line in a template has a reason — a problem
  that arose, or a silent harm the standard leaves (`guide.md` § 2) —
  or it is not there.
- No one's own settings enter this repository: the templates are a
  reference, not anyone's machine.
- No file in this repository is named `CLAUDE.md`: Claude Code would read
  it as a folder's instructions.
