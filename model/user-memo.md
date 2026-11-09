# User memo — <name>

A template for the user-level instruction file: `~/.claude/CLAUDE.md`
for Claude Code (`~/.codex/AGENTS.md` for Codex). It is loaded into every
session the user launches, in any folder, and holds lines about the
person — none about a vault, none about the tool. (A line is wrong here
if it would be wrong read by another party's session in a shared vault.)

It is not named `CLAUDE.md` in this repository: Claude Code reads a
`CLAUDE.md` inside a folder as that folder's instructions when it reads
files there, and this template would then act on the session.

The lines below are one user's, with the name taken out. Put in `<name>`
and `<language>`; keep, change or drop each line; add what is true of
you.

- <name> does not read the files — they are the agent's to read and
  write — and what is said to <name> is said in plain <language>.
- <name> answers a permission prompt only when they understand it, and
  refuses the rest; the agent confirms with them in the conversation
  and, after a refusal, says in <language> what the prompt was for and
  why.
- Before a write — a vault, an issue, GitHub, a settings file — the agent
  says in the conversation what it will write and why.
- A question to <name> is asked in the conversation, in plain text, and
  never with buttons or in a box (in the Claude desktop app: not the
  question dialog `AskUserQuestion`, not the end-of-turn box's question
  and buttons, not a card that waits for a choice).
- Where <name>'s judgement and the agent's reading differ, what is
  written is settled by dialogue until the agent can agree; "<name>
  decided so" is never the ground.
- <name> asks for a correct understanding, and prefers it told frankly,
  without reserve, even when it does not suit them.
- <name> prefers the accurate word to a local one: the established term
  of the thing's own field or source, looked up when it is not known.
