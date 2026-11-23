# agent-setup-guide

**If you are an agent helping a user set up their own harness** — the
settings and instructions by which their agent is held on their machine,
for any tool on any platform — read `guide.md` and follow it: it is the
procedure, and you build the harness for the tool you are, from that
tool's present docs. Where `setup/` has a file for the user's tool and
platform, read it as a dated reference — a route that worked — and
check it against the tool as it is. Talk with the user in their
language. Write
nothing in this folder: the user's own files are elsewhere, and what you
learn goes into the report the guide asks for (§ 5).

**If you are a vault's agent writing that vault's own procedures for its
users** — setting up a harness, changing it later, adding a git
repository inside the vault — read `vault-procedures.md`: a vault's
procedure holds for any environment; it states the vault's needs in no
tool's terms and rests on `guide.md` § 4.

The rest of this file is for anyone else.

## Files

| file | what it is |
|---|---|
| `guide.md` | the guide and the procedure, for any tool and platform: what a harness is, the layers, the test for a line of one's own, what to watch for, what the user decides, who builds the harness and from what, the steps of a setup in order (the application's own settings, the files, the check, each only where the user's route has it — no line for a repository nested in a vault, which each user adds later) and of a later change, the report back |
| `setup/` | a worked route for one tool and platform, dated, to be consulted and checked against the present tool: what it sets and how far it was tried, the steps as they went, each value with its reason, the entries for a later change, a lighter alternative where there is one: `codex-macos.md` (Codex in the ChatGPT desktop app on macOS), `claude-code-macos.md` (Claude Code on macOS). None for Claude Code on native Windows: never run on a machine |
| `environments/` | the evidence the files in `setup/` point to: what has been found about particular tools and platforms, dated and sourced, tried and read told apart: `claude-code-macos.md`, `claude-code-windows.md` (from the docs; not run on a machine), `codex.md` (the docs, trials in the ChatGPT desktop app on macOS), `github.md` (an agent's credential on GitHub: the kinds of token and their reach, who can make which, an organization, a transfer of a repository — the docs, and a trial with two accounts) |
| `model/` | the text of the templates for Claude Code: `settings.json` (the fragment for the user settings), `rules/harness.md` (a user rules file), `user-memo.md` (a user memo with placeholders, for `~/.claude/CLAUDE.md`), `windows/` (native Windows; kept apart, no sandbox there), `optional/` (a piece not in the default). The reason for each macOS line is in `setup/claude-code-macos.md` |
| `vault-procedures.md` | for a vault's agent: how a vault writes its own procedure so that it holds for any environment — what the vault states of its needs in no tool's terms, what is pointed to as already known, what it keeps from `guide.md`, how a user reaches it (two short sheets — the agent tool's own settings and the harness, which holds only what the user's route has to set and checks only what was set, then the vault's installation — each with a text to paste; the agent leads), a later change, adding a git repository inside the vault — a procedure of its own, apart from the first setup (with what each tool's harness needs there) |
| `README.md`, `LICENSE` | for people reading on GitHub; CC0 1.0 |

## Maintaining this repository

- Every file is in English: agents read it, and talk with each user in
  the user's language.
- The base holds for any environment: `guide.md`, and a vault's
  procedure written by `vault-procedures.md`, must let an agent set up
  whatever its tool and platform. A file in `setup/` is a dated record
  of a route that worked — consulted and checked, never what the base
  depends on. It ages; its date and how far it was tried stay on it.
- One fact in one place. A value and its reason are in the tool's file
  in `setup/`; the dated finding it rests on is in `environments/`;
  what is true of any tool is in `guide.md`. Text that moves is moved,
  not copied.
- A note in `environments/` is dated and sourced, and says of each
  point whether it was tried or only read; where a note and the tool's
  official docs differ, the docs win. A file in `setup/` states as
  tried only what was tried, with its conditions.
- A route in `setup/` is given as tried only after ordinary work has
  been done under it on the tool's present version — a session on a
  folder that holds an instruction file, past its first message — and
  not after its check of refusals alone. That test is the maker's,
  made before a user is given the route; a fault it finds changes the
  route, and is not answered by a further step in every user's check.
  Before a route is built on a feature, its vendor's own mark (Beta,
  experimental), the tool's open reports and what its users run are
  looked at (`guide.md` § 2).
- A value in `setup/` or a line in a template has a reason — a problem
  that arose, or a silent harm the standard leaves (`guide.md` § 2) —
  or it is not there.
- No one's own settings enter this repository: the templates are a
  reference, not anyone's machine.
- No file in this repository is named `CLAUDE.md`: Claude Code would read
  it as a folder's instructions.
