# A vault's own procedures: the base

For a vault's agent. A vault that other people use holds procedures for
them: how a user's harness is set up before the vault is first opened,
how it is changed later, and — where the vault holds other git
repositories — how one is added. Those procedures are the vault's own
documents, written by the vault's agent with the vault's places and
names, and carried out on each user's machine by that user's own agent.
A vault cannot assume that a user has anything but their agent tool and
this guide.

A vault's procedure holds for any environment: from it, a user's agent
can set up the user's harness whatever its tool and platform. It rests
on `guide.md` § 4, which names no tool, and adds what only the vault
knows — what its routine needs — in no tool's terms. The settings of a
tool are not in it: the user's agent finds them for the tool it is,
and where something is already known for that environment (`setup/`,
`environments/`) it consults that as a dated reference. This file says
what a vault states and what it leaves to the user's agent, and the two
sheets a user is handed (§ 1), how the procedure serves a later change
(§ 2), and what adding a repository inside the vault needs (§ 3). Dated
2026-11-23.

## What a vault may and may not do

- **A vault may say what it advises its users**, each point with its
  reason, in no tool's terms, and point to what is known for the
  environments they run. It is offered, never enforced: the harness is
  the user's (`guide.md` § 0), and a user and their agent may change
  any of it.
- **A vault states what its routine needs**, as facts the user's agent
  can work from: which commands a session runs (a pull at the start, a
  commit, a push, the vault's scripts, issues through a CLI), which
  hosts they reach, which of them write under `.git`. A harness that
  bounds commands will stop some of these, and how each is let through
  is the user's decision, with its cost (`guide.md` § 2.14).
- **A vault keeps no user's settings** and asks none to be committed
  (`guide.md` § 2.13).
- **A vault does not rely on a user's harness for its own rules.** What
  must hold for the vault is kept where it holds whatever the tool: its
  commit and push checks (`guide.md` § 2.12).

## 1. The first setup

When: before the user's agent is first opened on the vault (`guide.md`
§ 4, "When").

### What the procedure is

Two short sheets from which a user's agent can lead the user from
nothing to work in the vault, whatever its tool and platform: first the
agent tool's own settings and the harness, once for a machine; then the
vault's installation, in a session inside bounds already set and
checked ("How a user reaches it", below). The steps of the first and their order are
those of `guide.md` § 4, which hold for any tool; what the sheets add
is what only the vault knows. They are written in no tool's terms and
carry no tool's settings. What is handed to the user's agent is small;
the detail — each value's reason, what a later change needs — stays in
this guide as material the vault points to. The procedure is also what
is read again at a later change (§ 2).

**What the vault states** — its needs, as plain facts, once:

- what a session of its routine runs: the commands (a pull at the
  start, a commit, a push, the vault's scripts, issues through a CLI),
  the programs they need, which of them write under `.git`;
- the hosts they reach;
- what the routine needs of which repository — reading and writing its
  contents, its issues — and who owns that repository: a user's own
  account, an organization whose members the users are, or another
  person's account. Who owns it decides which kinds of credential a
  user can make for an agent (`environments/github.md`); a vault whose
  users are collaborators on another person's repository says which way
  it offers them;
- the git repositories nested in the vault, each by its path from the
  vault's top, and for each whether the agent or the user pulls and
  pushes it (§ 3);
- whether the vault's repository tracks files under a folder that an
  agent tool loads its own settings from (`.claude/`, `.codex/`,
  `.agents/`). A harness may keep such a folder read-only to the agent,
  and a pull that brings a change there can then fail half-way: the
  vault says how its routine pulls in that case (seen under one tool:
  `setup/codex-macos.md`, step 3);
- the vault's repository and the folder it is cloned into; where the
  record is kept — outside the vault;
- anything it asks a user to add to their settings beyond what the
  routine needs, with its ground by the test of `guide.md` § 2 — and
  that such a line holds wherever the user's settings do, which may be
  every folder the user opens, not the vault alone.

**What is already known, pointed to.** Where `setup/` has a file for an
environment the vault's users run, the procedure names it as a
reference — a route that worked, with its date — and may add what the
vault has itself seen there, saying where and when. It does not copy
that file's lines and is not written in that file's order or terms. A
user on that environment has their agent read the file and check it
against the tool as it is (`guide.md` § 4). A user on any other
environment is not stopped: their agent builds the same steps for its
own tool, says that the result was not tried before, and what it finds
is told to the vault's owner, so that the vault can point the next such
user to it.

**What it keeps from `guide.md`, unchanged**:

- the test for a line (`guide.md` § 2), and the user's right to change
  any line: what the vault advises is offered;
- that the agent leads the user, and that a session which leads is
  never missing (`guide.md` § 4, "The agent leads");
- the order — the application's own settings, the files, the lines for
  each nested repository where the tool needs them, the check — and the
  vault cloned only after the harness;
- the choice offered first, to leave it to the agent or to go point by
  point, and what still holds when it is left (`guide.md` § 4 step 4);
- the split between what is needed and what is advice, and the tone: a
  vault's procedure turns no advice into an order. A setup that is long
  or reads as a list of orders is put off or resented (`guide.md` § 4);
- the marks of what was tried: the procedure states as tried only what
  was tried, on which environment and when. Setting up from it on an
  environment with no file in `setup/` has not been tried by anyone
  yet (2026-11-22).

### How a user reaches it

A new user has the invitation to the vault's repository and an agent
tool, perhaps not yet a coding agent, and no harness. Their aim is to
begin working in the vault; a harness is not a thing they asked for,
and its words are not theirs. The procedure cannot begin inside the
vault: the vault is opened last.

It is in two sheets. A sheet is a short document the user reads in the
browser — the invitation accepted, whoever invited the user gives the
links to them (a README at the top of the vault's repository may lead
to them as well); an invited person reads them there, so they may live
inside the vault. It holds a few
acts of the user's own, said at its top, then a text to copy and paste
to their agent. From there the agent leads (`guide.md` § 4, "The agent
leads").

**Sheet 1 — the agent tool's own settings and the harness.** Once for a
machine.

1. **The user starts a session that is not on the vault's folder** —
   where the tool starts one with no folder chosen and gives it a
   working folder by itself, that: the user has no setup folder to make
   or to understand. Not the folder that holds the user's vaults: an
   agent opened there may write into every vault under it.
2. **The user pastes the sheet's text.** Pasted into a chat that cannot
   work on the user's machine, it has that agent say that one which
   can is needed — Claude Code, Codex or another — and how to go on:
   the same text, pasted there.
3. **The agent leads**, one act at a time: the tools the routine needs;
   this guide brought to hand as a local copy; the harness built by
   `guide.md` § 4 for the tool it is — the application's own settings,
   the files, the check — reading, where there is one, the file in
   `setup/` for its environment. What of the vault's needs goes into
   the harness's own files — the routine's programs and hosts, the
   lines for each repository nested in the vault — is in this sheet's
   text, so that a settings file is written once, by a session that
   can still read it back; a session inside the bounds may not be able
   to read its own settings.
4. **The check is part of this sheet.** Before it ends, what should be
   refused — a write outside the folder a session is opened on, a
   connection to a host that is not allowed — is seen refused, and the
   leading session keeps the lead through it: it alone can mend the
   settings, and between the two sheets there must be no stretch with
   no one leading. Where the tool tries new settings only in a new
   session, the leading session gives the user a short text for that
   session, which only tries these things and reports — it does not
   take the lead — and the user brings its answer back. What is not
   refused is mended and tried again.
5. **It ends when the check has passed.** The agent says so in the
   user's words, and says what comes next in a way that needs no
   knowledge of the sheets: that there is a second page, by its title
   and its address, which the user opens to do the acts at its top.
   Nothing is carried from this session to the next — it writes no
   text for the user to hand over. The vault is not yet on the
   machine.

**Sheet 2 — the vault's installation.** Once for each vault, in a
session inside bounds that the first sheet put in and checked.

1. **The user makes the vault's folder and opens the agent on it** —
   the sheet names the folder at its top. An act of the user's own: a
   command handed over for so small a thing is more trouble to a user
   than the act.
2. **The user pastes the sheet's text, and the agent leads**: the
   credential the agent uses inside the bounds for the vault's
   repository — the user's hands, never shown to the agent; the vault
   cloned into that folder by the user's own command (some harnesses
   keep the agent from creating a repository's Git files at all); the
   check on the vault; then the vault's own first steps, by the vault's
   documents, which are at hand from the clone on.
3. **Its text is complete by itself.** It relies on nothing carried
   from the first sheet's session. And it carries what its agent needs
   for its own steps on a route the vault has seen — how the credential
   is stored under that harness, say — as a dated reference: a session
   inside the bounds may be unable to reach the copy of this guide that
   the first session brought.

Why two, and why in this order. Before the harness, an agent that
cloned the vault would be one with no bounds, using the user's
credential, with the vault in its hands: so the harness first. Kept
apart, each sheet has one aim the user can follow and one session that
leads it from its first act to its last; and each agent has less to
read and do — the more a session reads and does, the more it fails and
the less it attends to the user. The cut falls where the session
changes anyway: bounds hold only in a session begun after they are set.
The step from the first sheet to the second is not a hand-over of the
lead: the first is finished and checked, the second begins by its own
text.

An earlier form of this entry (2026-11-21) — one text, a setup folder
of its own, the check in further sessions, the vault cloned at the end
of the same procedure — was run once by a user, on Codex in the ChatGPT
desktop app on macOS (2026-11-22), and not to its end: the first
session led; the sessions the work then moved to were handed single
requests and led nothing, and the user carried messages between them.
The two sheets answer that (settled 2026-11-22). They were run the
next day on the same route as far as the end of the first sheet, and
that run changed two things, now above: the first sheet's agent named
the second by its title alone, and the user did not know where to go;
and the refusals had been left to the start of the second sheet, where
the session that finds a fault cannot mend it and no one leads between
the two. In the present form the entry has not yet been run; the files
in `setup/` mark what is untried on their routes.

### The text to paste

Each sheet's text is the vault's word, handed to the user, who gives it
to their own agent. It is short.

How it speaks:

- **Not in the user's first person.** Its head says that it was
  prepared on the vault's side, that the person who pastes it did not
  write it, and that this person may neither know what it asks nor ask
  when something is unclear. A text that says "I" leaves open who
  speaks, and read as the user's own words it tells the agent that
  this person knows what they want — the opposite of what an agent
  that is to lead should assume.
- **It names no person of the vault's side.** To the agent it does not
  matter who they are; on a problem the user asks whoever invited them.
- **Of the user it states only what is fact.** What it takes for
  granted — that the first sheet is done, that the user is a member of
  the repository's organization — it states as what it assumes, and
  the agent says where an assumption does not hold. It does not
  assume that the user knows the sheets' titles, or the words of the
  text itself.
- **It need not be in the user's language.** The sheet then says which
  part the user reads — the acts at its top — and that the text need
  not be read; and the text tells the agent which language to speak to
  the user.

It holds:

- the aim, in the user's terms: to begin working in a named vault —
  and, for sheet 1, that this part readies the agent tool itself,
  before the vault;
- the agent's part: it leads to the end of this sheet, in the user's
  words, one act at a time; if the work moves to another session, the
  lead is handed over as the lead (`guide.md` § 4);
- where the detail is: this guide's public address. The text names no
  tool: the agent that receives it works for the tool it is;
- sheet 1: the vault's needs that go into the harness (above), as plain
  facts, in no tool's terms; that the set the agent proposes may be
  taken as it stands or gone through point by point, and that any
  value may be changed by the user;
- sheet 1, at its end: the second sheet's title and address, for the
  agent to give to the user;
- sheet 2: the vault's repository and who owns it, what the routine
  needs of it, the folder, which of the vault's documents the agent
  goes by once the clone is there, and what its agent needs on a route
  the vault has seen (above);
- what the agent does not do: clone the vault, write what its tool
  leaves to the user's hands, post anything;
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
   announced to the users as a change in the vault's needs, in no
   tool's terms — the new host, the new program, the new repository's
   path; each user's agent turns it into the entry of its own tool.
2. **What a session does when the harness stops the routine**: it does
   not work around the stop and does not change settings itself; it
   reports what was wanted, where and why.
3. **Where the change is made** — by tool (`guide.md` § 4, "Later"): in a session of its own, outside the vault, where the agent can
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

### What is known, tool by tool

As found on the dates given in `environments/` and `setup/`: a
reference for the user's agent, checked against the tool as it is. For
any other tool, the test below.

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
  macOS). Four literal `read` lines for each nested repository — five
  where the user keeps `.claude` read-only (`setup/codex-macos.md`,
  steps 3 and 4) — written before it is cloned. No pattern can stand in for them: a `read` glob
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

Like the rest of a vault's procedure (§ 1) it is written in no tool's
terms and carries no tool's lines: it says what the vault does and
announces, and what each user's agent sees to for its own tool. The
order matters where a tool needs lines for each repository: a
repository that exists before its lines is open until they are written.

1. **The place.** Where in the vault the repository goes is the vault's
   to say; it has no meaning to the harness. The procedure announces
   the repository's path from the vault's top.
2. **The vault's own Git ignores it.** An entry in the vault's
   `.gitignore`, committed with the vault, before the clone. Unignored,
   `git add` in the vault records the nested repository as a link, and
   the vault's checks may read its files as the vault's.
3. **The user's harness, before the clone.** Where the user's tool
   needs lines for each nested repository, they go in before the clone,
   and the work goes on in a new session. Where it needs none, the
   agent checks once that the settings that hold at any depth are in,
   and says so if they are not. Which of the two is the case, and what
   the lines are, the user's agent finds for its own tool — from "What
   is known, tool by tool" where the tool is there, by the test at
   depth where it is not.
4. **The clone is the user's**, in their own terminal: a harness may
   keep the agent from creating a repository's Git files, or from the
   host and the credential a clone needs.
5. **Who pulls and pushes it.** This depends on the repository, not on
   the harness: the procedure says, for each, whether the agent or the
   user does. If the agent is to, the credential it uses inside its
   bounds must cover that repository; a repository an agent must never
   push is protected at its remote.
6. **The check**, in a new session on the vault: the list of nested
   repositories (`find . -mindepth 2 -name .git -not -path './.git/*'
   -prune`) against what the user's harness protects, where the tool
   protects each by its path; and one write to Git's `config` in the
   new repository refused, with its message.
7. **Moving, renaming, removing.** Where a tool protects a repository
   by its path, the new lines come first when it moves or is renamed. A
   harness may also keep the agent from moving or removing the folders
   that lead to a protected repository; the user does it then.
8. **What stays open**, said in the procedure itself so that no user
   assumes more: a repository with no lines, under a tool that needs
   them; the vault's own hook programs in ordinary folders; skills;
   whatever the test at depth showed for the tools its users run.

### Who writes what

- **The vault** writes the procedure and its ignore entry, and announces
  the new repository's path. It gives no tool's lines.
- **Each user's agent** finds what its own tool needs for the
  repository, and carries the procedure out on that user's machine,
  saying each step before it is done.
- **The user** applies what the tool leaves to them — a settings file
  the agent may not write, the clone.
