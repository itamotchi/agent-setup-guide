# A vault's own procedures: the base

For a vault's agent. A vault that other people use holds procedures for
them: how a user's harness is set up before the vault is first opened,
how it is changed later, and — where the vault holds other git
repositories — how one is added. Those procedures are the vault's own
documents, written by the vault's agent with the vault's places and
names, and carried out on each user's machine by that user's own agent.
A vault cannot assume that a user has anything but their agent tool and
this guide.

A vault's procedure is not composed anew. It is `guide.md` § 4 and the
file in `setup/` for the tool its users run, with the vault's values
put in: the same steps in the same order, each value with its reason.
This file says what is put in and what is not, how a user reaches the
procedure (§ 1), how it serves a later change (§ 2), and what adding a
repository inside the vault needs (§ 3). The facts per tool are in
`environments/`, each marked tried or read; the way to judge a setting
is in `guide.md` § 2. Dated 2026-11-21.

## What a vault may and may not do

- **A vault may give its users a standard**: one set of settings for the
  tool its users run, each line with its reason. It is offered, never
  enforced: the harness is the user's (`guide.md` § 0), and a user and
  their agent may change any of it.
- **A vault states what its routine needs**, as facts the user's agent
  can work from: which commands a session runs (a pull at the start, a
  commit, a push, the vault's scripts, issues through a CLI), which
  hosts they reach, which of them write under `.git`. A harness that
  bounds commands will stop some of these, and how each is let through
  is the user's decision, with its cost (`guide.md` § 2.14).
- **A vault keeps no user's settings** and asks none to be committed
  (`guide.md` § 2.13). Lines for a user's own settings files are given
  in the procedure as text.
- **A vault does not rely on a user's harness for its own rules.** What
  must hold for the vault is kept where it holds whatever the tool: its
  commit and push checks (`guide.md` § 2.12).

## 1. The first setup

When: before the user's agent is first opened on the vault (`guide.md`
§ 4, "When").

### What the procedure is

`guide.md` § 4 and the tool's file in `setup/`, adjusted to the vault.
One document for each tool the vault's users run, or one with a part
for each. It is both the setup and what is read again at a later change
(§ 2), so it keeps each value's reason: a user and their agent can then
tell which line to change.

**What is adjusted** — the vault's values, put where the tool's file
names them ("The values a vault puts in"):

- the tool its users run, and so which file in `setup/` is followed;
- what the vault's routine runs and reaches: the commands of a session
  (a pull at the start, a commit, a push, the vault's scripts, issues
  through a CLI), the programs they need, the hosts they reach;
- the repository a token is made for, who owns it, and the permissions
  the routine needs on it, where the tool's file uses a token. Who owns
  it decides whether a user can make the token at all: a token limited
  to one repository can be made by its owner or by a member of the
  organization that owns it, not by a collaborator on another person's
  repository (`environments/github.md`). A vault whose users are
  collaborators says which way it offers them instead;
- the git repositories nested in the vault, each by its path from the
  vault's top (§ 3);
- where the vault's standard differs from the tool's file, and why —
  each difference with its ground, by the test of `guide.md` § 2;
- the name of the setup folder, and where the record is kept — outside
  the vault.

**What is not adjusted**:

- the test for a line (`guide.md` § 2), and the user's right to change
  any line: the standard is offered;
- the order — the application's own settings, the files, the lines for
  each nested repository, the check — and the vault cloned only after
  the harness;
- what stays the user's own hands, as the tool's file lists it;
- the marks of what was tried. Where the tool's file says a step was not
  tried, the vault's procedure says so too, until the vault has tried it
  — and then it says where and when;
- the choice offered first, to leave it to the agent or to go point by
  point, and what still holds when it is left (`guide.md` § 4 step 4);
- the split between what is needed for the set to hold and what is
  advice, and the tone: a vault's procedure turns no advice into an
  order. A setup that is long or reads as a list of orders is put off
  or resented (`guide.md` § 4).

### How a user reaches it

A new user has the invitation to the vault's repository and an agent
tool, perhaps not yet a coding agent, and no harness. The procedure
cannot begin inside the vault: the vault is opened last.

1. **The invitation is accepted, and the user is given a link to the
   vault's introductory document.** A README at the top of the vault's
   repository leads to it. An invited person reads it in the browser, so
   it may live inside the vault.
2. **The document holds a text to copy and paste to the agent.** The
   text carries the vault's values, so the agent that sets up needs no
   read of the vault. Pasted into a chat that is not a coding agent, it
   has that agent say that Claude Code or Codex is needed and how to go
   on: the same text, pasted there.
3. **The first session is opened on a setup folder of its own.** The
   vault's folder will not do: a clone wants its target empty or
   missing, and a vault should not be open to an agent before there is
   a harness. Nor will the folder that holds the user's vaults: an
   agent opened there may write into every vault under it, those still
   to come among them. The setup folder is also where a later change is
   made when it is made outside the vault. The agent says what will
   happen and offers the two ways.
4. **The tools and the GitHub login first** — `git`, `gh`, `gh auth
   login` — one command at a time, run by the user in their terminal.
5. **The agent brings this guide to hand, as a local copy, and follows
   the tool's file with the vault's values.** The application's screens,
   a token, the paste of a settings file are the user's hands.
6. **The check**, in a new session.
7. **The vault is cloned after the harness, by the user's own command.**
   Before the harness, the agent that cloned it would be one with no
   bounds, using the user's credential, with the vault in its hands.
   After it, some harnesses keep the agent from creating a repository's
   Git files at all.
8. **The agent is opened on the vault's folder**: the vault's own first
   steps follow.

This entry was settled on 2026-11-21 and has not yet been run from its
first step by a user; the tool's file marks what is untried in it.

### The text to paste

It is the vault's word, handed to the user, who gives it to their own
agent. It holds:

- what it is: the setup of the user's own harness, before a named vault
  is opened, by this guide — with the public repository's address — and
  which file in `setup/` is followed;
- where the session is opened: the setup folder's name, and that it is
  not the vault's folder;
- the vault's values (above), as plain facts;
- that the agent offers first to leave it to the agent or to go point
  by point, and that any value may be changed by the user;
- what the agent does not do: clone the vault, write a file the tool's
  file gives to the user's hands, post anything;
- nothing secret: no token, no password. A token is made by the user and
  never shown to the agent.

## 2. A later change

Most lines of a user's own come after the first setup: a line is added
when a problem arises, and problems arise in use (`guide.md` § 2, the
test; § 4, "Later"). The vault's procedure is the document read then,
and says so at its top. For that it holds:

1. **The occasions that are the vault's doing**: the routine changed (a
   new script, a new host, a new command), a repository was added inside
   the vault (§ 3), a rule of the vault moved to another layer. Each is
   announced to the users with what their harness needs for it — the
   entry, as the tool's file lists it ("Later: the entry for each
   occasion"), with the vault's value in it.
2. **What a session does when the harness stops the routine**: it does
   not work around the stop and does not change settings itself; it
   reports what was wanted, where and why.
3. **Where the change is made** — by tool (`guide.md` § 4, "Later"; the
   tool's file): in a session on the setup folder where the agent can
   write some of its own settings or instructions; where the tool
   refuses the agent all of them, the change may be prepared in a
   session on the vault, which can read this procedure, and the user
   applies it by hand.
4. **The steps** (`guide.md` § 4, "Later"): say what arose; the smallest
   change that answers it, or none; back up, write or have the user
   apply, read back; check; record the line with its reason and date.
   A line is taken out the same way.

## 3. Adding a git repository inside the vault

A vault may hold another git repository in a subfolder — a site's code,
say — so that the vault's work can read and edit it inside the same
bounds as the vault.

### Why the harness is concerned

Some files in a repository are read and run later by another program,
outside any sandbox, with the user's rights:

- Git's `config` — it can name commands (`core.fsmonitor`,
  `core.hooksPath`, a credential helper, an alias that begins `!`);
- Git's `hooks`;
- each agent tool's project folder — its settings, hooks and skills
  (`.claude/`, `.codex/`, `.agents/`).

An agent that can write one of these can have a program run the next
time the user, or an agent's command outside the sandbox, uses Git or
opens that folder. Agent tools protect these files, but the protection
may reach the top of the opened folder only. In a nested repository the
same files were writable, on one Mac, under Claude Code and under Codex,
each in its own way, with no prompt (`environments/claude-code-macos.md`,
`environments/codex.md`).

### What each tool's harness needs

- **Claude Code on macOS.** Nothing for each repository. Five lines in
  the user settings hold at any depth — two deny rules on the file tools
  for Git's hooks and config, three sandbox write-denies for a nested
  `.claude`'s settings files and hooks (tried; the lines are in
  `model/settings.json`). A repository cloned into the vault later is
  covered with no new line. Left open: skills in a nested `.claude`;
  hook programs a vault keeps in ordinary folders; a submodule or a
  worktree (not tried).
- **Claude Code on native Windows.** No sandbox: the two deny rules bind
  the file tools, and a command can still write these files (read, not
  tried).
- **Codex with a permission profile** (the ChatGPT desktop app on
  macOS). Five literal `read` lines for each nested repository, written
  before it is cloned. No pattern can stand in for them: a `read` glob
  is rejected (read), and a `deny` glob with `**` stops the agent from
  removing or moving any folder in the vault (tried). A repository with
  no lines is open, with no sign.
- **Codex under a preset, with no profile.** Its own protection is at
  the top only (the source, read; not tried under a preset), and a
  preset has no place for a line: a nested repository is open. Say so to
  the user.
- **Another tool.** Run the test below; do not assume.

### The test at depth

With the user's yes, in a session opened on the vault: make a throwaway
repository in a subfolder (`git init`), then try, as the agent, in it
and at the vault's top —

- `git config <key> <value>`;
- a new file in `.git/hooks/`;
- a new file in each tool's project folder (`.claude/`, `.codex/`,
  `.agents/`), by a shell command and by the tool's file-editing tool.

Record each result with its message, and remove what was made. A write
that passes at depth and is refused at the top is the gap.

### What the procedure holds, in order

The order matters where a tool needs lines for each repository: a
repository that exists before its lines is open until they are written.

1. **The place.** Where in the vault the repository goes is the vault's
   to say; it has no meaning to the harness.
2. **The vault's own Git ignores it.** An entry in the vault's
   `.gitignore`, committed with the vault, before the clone. Unignored,
   `git add` in the vault records the nested repository as a link, and
   the vault's checks may read its files as the vault's.
3. **The user's harness lines, for the tool that user runs** — before
   the clone:
   - Codex with a profile: the five lines for the repository's path
     (`setup/codex-macos.md`, step 4), in the user's
     `~/.codex/config.toml`. The agent shows the
     lines and how to open that file (the folder is hidden); the user
     takes a backup copy, pastes the lines and saves, with the app
     running — no quit, no restart (tried on one version, macOS). The
     lines hold from the next new chat; a chat already open keeps its
     earlier permissions, so the work goes on in a new one.
   - Claude Code: no line for the repository. The agent checks once that
     the user's settings carry the lines that hold at any depth, and
     says so if they do not.
4. **The clone is the user's**, in their own terminal. With its lines
   in, Codex cannot create that repository's Git files (tried); under Claude
   Code's sandbox a clone needs a host and a credential the sandbox
   hides (inferred from what a push met; not tried).
5. **Who pulls and pushes it.** This depends on the repository, not on
   the harness: the procedure says, for each, whether the agent or the
   user does. If the agent is to, the credential it uses inside its
   bounds must cover that repository; a repository an agent must never
   push is protected at its remote.
6. **The check**, in a new session on the vault: the list of nested
   repositories (`find . -mindepth 2 -name .git -not -path './.git/*'
   -prune`) against the lines, where the tool has lines for each; and
   one refused write in the new repository — `git config <key> <value>`
   under Codex, an edit of `.git/config` with the file tool under Claude
   Code — with its message.
7. **Moving, renaming, removing.** The lines name a path: when a
   repository moves or is renamed, the new lines come first. Under Codex
   the folders that lead to a listed repository cannot be moved or
   removed by the agent; the user does it.
8. **What stays open**, said in the procedure itself so that no user
   assumes more: a repository with no lines; the vault's own hook
   programs in ordinary folders; skills; whatever the test at depth
   showed for the tools its users run.

### Who writes what

- **The vault** writes the procedure and its ignore entry. The lines for
  each tool are given in the procedure as text.
- **Each user's agent** carries the procedure out on that user's
  machine, saying each step before it is done.
- **The user** applies what the tool refuses the agent — a settings file
  the agent cannot write, the clone. Under a Codex profile the agent
  could not write its own `config.toml` (tried); whether a chat switched
  to ask the user for each approval can, with the user's approval, was
  not tried.
