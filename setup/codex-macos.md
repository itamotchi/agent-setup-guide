# Codex in the ChatGPT desktop app on macOS: the setup

A worked route: how the harness of Codex in the ChatGPT desktop app on
macOS was set up on a test bed in November 2026, in order, each value
with its reason. A reference for the user's agent, which builds the
harness itself by `guide.md` § 4: read this first, check each value
against the app as it is and its present docs — the app changes within
weeks — take what still holds, and say what does not. It is not a
procedure to apply unread, and a vault's procedure is not made of it
(`vault-procedures.md`).

Three files work together. `guide.md` holds the procedure, for any
tool: how a line is judged, what the user decides, the steps. This file
records what was set for this tool on that route, and why. The dated findings
and trials each value rests on are in `environments/codex.md`, named
here by section as "→ *section*". A reference, never a requirement: the
user may change any value (`guide.md` § 3), and where this file and the
docs for the user's version differ, the docs win.

## What it sets, and how far it was tried

**What the user gets.** Nothing outside the folder Codex is opened on
is read or written — held by the sandbox, not by a model's judgement
(`guide.md` § 3.1). The commands the agent runs reach GitHub and no
other host; looking at the web — web search and the built-in browser —
still works, and is not bound by that list. A vault's routine — a
pull, a commit, a push, its issues through `gh` — runs inside those
bounds, with no approval asked. Git's `config` and `hooks` and Codex's
own project folders (`.codex`, `.agents`) are kept from the agent, at
the top of the opened folder and in each nested repository that has its
lines.

**Tried**: on one Mac, in the ChatGPT desktop app (versions 26.1118.11504
and 26.1111.71704, as the app showed them), on 2026-11-19 and
2026-11-20, in a Business workspace, with Codex opened on one vault.
On that Mac web search was set to live and the built-in browser to open
pages without asking (step 9); with the app's defaults for those two the
set was not tried. On that Mac the profile also kept `.claude`
read-only, at the top and in each nested repository; that line is
optional here since 2026-11-21 (step 3), so the set below is the one
tried, less that line.

**Followed from its first step, 2026-11-22** (the app 26.1117.41037,
the user's `~/.codex` set aside first; the user a member of the
organization that owns the vault's repository; two runs, from a vault's
procedure made of an earlier form of this file, neither as far as the
clone). Seen: the profile went in by the user's hand, the whole file at
once, and showed after one restart of the app (step 3); a member's
token, approved by the organization's owner, read the repository inside
the profile; a write inside the folder passed, a read outside and
`example.com` were refused, and `git ls-remote` by the repository's
address passed (step 7). What the runs showed of the route itself — a
setup folder the user did not follow, the leading lost as the work
moved from chat to chat — changed the entry: a setup is now two
procedures (`guide.md` § 4, "The agent leads" and "The harness, then
the vault"; `vault-procedures.md` § 1). Of the steps below, 1 to 3, 6
and the first table of step 7 are the harness; steps 5 and 8 and the
second table are a vault's installation; step 4 is neither — it is
done later, each time the user adds a repository inside the vault. A
step a run corrected says so.

**A third run, 2026-11-23**, in the two-sheet form (the same app; the
model GPT-6.2 Sol with reasoning set to its highest, the app's default
being medium): the harness's half was led from a chat with no folder
chosen to its end — the tools and the GitHub login from a logged-out
state, the file applied, the rules file written. Read afterwards, the
file parsed and held the profile and the lines of every nested
repository as given. The agent had departed from the set as it then
stood in two places, unsaid before the save; each was tried afterwards
in a new chat under those settings (→ *A setup followed from its first
step*, the third run). **The built-in browser's three lines were left
out**, measured by the vault's routine, which has no browser in it:
the built-in browser did not start, and web search worked. They are in
the set for the app, not for a routine (step 3). **Git's credential
helper was given by a file that `GIT_CONFIG_GLOBAL` names**, in place
of `GIT_CONFIG_COUNT` pairs, because the app passes pairs of its own
in those variables: Git read the user's name and mail address and the
helper, and `git ls-remote` on the private repository passed with a
member's token. That form is the set's since (step 3). At the end the
agent named the next sheet by its title alone, and the user did not
know where to go; the refusals had been left to the second sheet. Both
changed the entry: the check is now part of the harness's half, led by
the chat that set the file up, and nothing is carried between the
halves (`vault-procedures.md` § 1). And the lines of a vault's nested
repositories, which that run wrote at the first setup — forty-four,
for eleven repositories, none on the machine — are no longer part of
it: a user's clone starts with none, and the lines of one are written
when the user adds it (step 4).

**Not tried**:

- this file followed to its end by a user who starts with nothing, in
  the present form: the check led by the chat that set the file up,
  the clone, a nested repository, the check on the vault. On the test bed
  the settings were reached over two days, in another order, and the
  configuration of step 3 was written by a session of another tool;
- a real push (a dry run passed); a pull or a push with a member's
  token; a commit, a pull or a push with Git's helper given by the
  file of step 3 (the pull and the push dry run of 2026-11-20 were
  made with the pairs);
- the CLI, another platform, another version of the app.

Each further untried point is marked where it stands. Say this to the
user before they choose how to proceed (`guide.md` § 4).

**What is needed, and what is advice.** The set holds with these: one
switch in the app (step 2), the profile (step 3) — whole: its lines
are the bounds and what keeps the app's own functions working inside
them, and neither kind is measured by a vault's routine — the token
(step 5), the check (step 7); and, later, the lines of a repository
when the user adds one inside the vault (step 4). The rest — the other switches of step 2, the
forbidden rules of step 6 — is recommended, each with its reason, and
can wait. Nothing here is forced: the harness is the user's.

**The user's own hands**, even when the rest is left to the agent: the
app's screens (step 2); the paste of `config.toml` (step 3); the token
(step 5); the clone of the vault (step 8); and leaving the permission
choice under the composer alone — a preset chosen there leaves the
profile for that chat.

## Where a vault's needs go on this route

A vault states its needs in no tool's terms (`vault-procedures.md`
§ 1); on this route they went here. A user with no vault's procedure
settles them with their agent.

| value | where it is used |
|---|---|
| the programs the vault's routine runs (`git`, `gh`, a script's interpreter) | step 3, the named reads |
| the hosts the routine reaches | step 3, the network |
| a git repository added inside the vault, by its path from the vault's top — when a user adds it, not at the first setup | step 4 |
| the repository the agent's token covers, who owns it (the user, an organization whose members the users are, or another personal account), and the permissions the routine needs on it | step 5 |
| the vault's repository, for the clone | step 8 |
| where the record is kept | step 8 |

## 1. Before the harness

1. **A chat with no folder chosen.** Started so, a chat is given a
   working folder by the app itself, under `~/Documents/Codex/` (seen
   2026-11-22; the place is a setting, under General). The user makes
   no folder for setting up: in the runs of 2026-11-22 a setup folder
   made for the purpose was a notion the user did not follow. Not the
   vault's folder: a vault is opened only when there is a harness. Not
   the folder that holds the user's vaults: an agent opened there may
   write into every vault under it. The harness's half was led from
   such a chat on 2026-11-23.
2. **The first chat.** A folder that is not under version control opens
   read-only (the docs; → *From the Codex docs*). Not tried: how many
   approvals this step then takes, and which permission choice under the
   composer suits a newcomer here — under Ask for approval every command
   that leaves the sandbox is a prompt to the user; under Approve for me
   a reviewer model answers in the user's place. The agent's own
   commands in this procedure are reads (where the programs are, which
   files exist); what writes outside the folder or connects is the
   user's hand, or one approval that the agent explains first.
3. **Say what will happen**, and offer the two ways: leave it to the
   agent, or point by point (`guide.md` § 4).
4. **The tools and the GitHub login**: `git`, `gh`, `gh auth login` —
   run by the user in their own terminal, one command at a time, the
   agent giving each. The login is the user's own: it serves their
   terminal and the clone of step 8, and `gh` is also what stores the
   token of step 5. Codex does not use this login inside the bounds set
   below.
5. **This guide, as a local copy** in the chat's working folder — a
   clone of the public repository, or its ZIP downloaded in the browser
   and unzipped. A local copy, not pages read through the web: the
   configuration's text matters to the letter. In the runs of
   2026-11-22 the agent offered both and proposed the clone, which
   passed; how many approvals it took was not recorded.

## 2. The app's own settings

They come before the file, because a choice made in the app can
override it (→ *The ChatGPT desktop app on macOS*, "The permission
control"). The agent cannot see these screens: it names a switch and
says what it is for; the user looks, and sets it if they agree.

Keep this step short, and force nothing. Only the first group is needed
for the set to hold. The rest is advice with a reason, said briefly; a
user may leave any of it, now or for good, and the agent then says in a
sentence what that leaves open.

The app changes. Names, defaults and what a switch does move between
versions; this list was read on one version, on a Mac whose settings
were no longer the defaults, so most defaults are not known here — the
few seen in a fresh state on 2026-11-22 are marked. The
user's agent is better placed than this file: it reads the docs for the
user's version, asks what the screen shows, and where that differs from
what is written here it goes by what it finds.

**Needed — one switch, and two things left alone.**

- **General, Full access: off.** That choice has no sandbox and no
  approvals; with the switch off it is absent from the menu under the
  composer (seen). In a fresh state the switch was **on** (seen
  2026-11-22, 26.1117.41037): the user turns it off. If it is already
  off, there is nothing to do.
- **The permission choice under the composer is left alone**, in every
  chat once step 3 is done. It shows the profile's name. A preset chosen
  there (Ask for approval, Approve for me) leaves the profile for that
  chat, the file unchanged (tried). The docs say that selecting a
  Daybreak model can switch the control by itself: look at it when a
  chat starts. Nothing keeps this choice but the user.
- **The Configuration screen's approval, sandbox and network switches
  are left alone**: under the profile these come from the file (step 3).
  The screen opens on the scope Work; Codex's switches are under the
  scope Codex (seen 2026-11-22, in a fresh state: approval on request,
  sandbox read-only, web search cached).

**Outside the profile — one sentence to the user.** The profile bounds
the commands the agent runs. It does not bound what acts as the user
elsewhere: computer use, the Chrome extension, passwords saved in the
app, control of the Mac from another device, a GitHub connection in
ChatGPT, cloud tasks. Each takes an act of the user's to begin — an app
allowed, an extension installed, a login saved, an account connected
(as read on the test bed; the defaults are not known here). The advice
is to leave them off while the vault is in use; a user who has one on
decides knowing that the profile does not reach it.

**Recommended** — none is needed for the profile to hold:

| switch | why |
|---|---|
| Codex memories off (the docs: off by default) | The docs: "a helpful recall layer, not ... the only source for rules that must always apply". |
| The "Codex" custom-instructions field empty | Undocumented, and it also reaches Codex Cloud; the documented place for the user's own instructions is `~/.codex/AGENTS.md`. |
| Notifications for permission requests and questions on | A prompt that does reach the user is seen. |
| Plugins the work does not use off | One thing less that can act. |
| Browser: history access off, site tools (WebMCP) off, downloads and uploads asked | The docs: history "can include internal URLs, search terms, and other sensitive information". |
| Git: always force push off | It rewrites a shared vault's history. |
| Import not run | Another tool's settings and instructions are that tool's. |

**Left at the app's default**: web search (the docs: cached by default)
and whether the built-in browser asks before it opens a page. Both can
be loosened later, when the need is met (step 9). Keyboard shortcuts,
the send key and appearance are the user's taste. Not judged on the
test bed: the bundled Node.js and Python ("Codex dependencies"), Plugin
Management, worktrees, hooks of the user's own (it had none).
→ *The ChatGPT desktop app on macOS*, "Other settings that bear on
Codex".

## 3. `~/.codex/config.toml`: the profile

The set proposed: the test bed's, less one line (`.claude` read-only,
now optional — below), and with Git's credential helper given by a
file since 2026-11-23 (below). The paths are that Mac's:
the agent finds each on the user's — where `git`, `gh` and the routine's
other programs are, which Git configuration files exist — and puts in
the user's home folder for `<name>`.

```toml
default_permissions = "workspace-fence"
approval_policy = "on-request"
approvals_reviewer = "auto_review"

[permissions.workspace-fence.filesystem]
":root" = "deny"
":minimal" = "read"
":tmpdir" = "deny"
":slash_tmp" = "deny"
"/Library/Developer/CommandLineTools" = "read"
"/opt/homebrew" = "read"
"/System/Library/OpenSSL" = "read"
"~/.gitconfig" = "read"
"~/.gitignore_global" = "read"
"~/.config/gh-codex" = "read"
"/private/tmp/codex-browser-use" = "read"

[permissions.workspace-fence.filesystem.":workspace_roots"]
"." = "write"
".git" = "write"
".git/config" = "read"
".git/hooks" = "read"
".codex" = "read"
".agents" = "read"
# none at the first setup. Later, step 4: four lines for each git
# repository the user adds inside the folder

[permissions.workspace-fence.network]
enabled = true

[permissions.workspace-fence.network.domains]
"github.com" = "allow"
"api.github.com" = "allow"

[permissions.workspace-fence.network.unix_sockets]
"/private/tmp/codex-browser-use" = "allow"

[features]
network_proxy = true

[shell_environment_policy.set]
GH_CONFIG_DIR = "/Users/<name>/.config/gh-codex"
GIT_CONFIG_GLOBAL = "/Users/<name>/.config/gh-codex/gitconfig"
```

and the file that last line names, `~/.config/gh-codex/gitconfig`:

```
[include]
    path = ~/.gitconfig

[credential "https://github.com"]
    helper =
    helper = !/opt/homebrew/bin/gh auth git-credential
```

The reasons:

- **`default_permissions`** applies the profile to every chat, on
  whatever folder Codex is opened. The approval keys are those tried;
  under the profile a request to run a command outside the sandbox ran
  inside it all the same, with no prompt: nothing left it.
  → *A permission profile with the routine inside*.
- **No `sandbox_mode`, no `[sandbox_workspace_write]`.** The docs: a
  profile or those, not both; either of them in any loaded file makes
  Codex use the older settings. If the user's file has them, they are
  taken out in the same edit. They are the keys the Configuration
  screen's sandbox and network switches stood for on the test bed. Not
  tried: what that screen shows once the profile is in, and whether a
  switch touched there writes them back.
- **`:root` deny, `:minimal` read**: nothing outside is read or written
  unless named. This is what stops a write outside the folder, by any
  program, escalated or not. → *An agent wrote outside its folder*;
  *A permission profile with the routine inside*, "A fence with no
  folder named".
- **The named reads** are of two kinds. What the routine's programs
  need: the Command Line Tools (`git`), Homebrew (`python3`, `gh`),
  Git's own configuration for the commit author. The agent reads the
  user's `~/.gitconfig` first and says so if it holds a credential or
  a helper: the file becomes readable to every chat. And what the app
  itself needs: the OpenSSL file, which the built-in browser's helper
  reads.
- **The built-in browser's three lines are the app's, not a
  routine's**: the OpenSSL read, the read on
  `/private/tmp/codex-browser-use` and the `unix_sockets` allow on it.
  They add no permission to a vault's work; they put back a function
  of the app that this profile's own denies take away — in every chat
  on the Mac, whatever folder it is opened on. So they are not weighed
  against what a vault's routine uses. Left out, the built-in browser
  did not start, the helper stopping at the OpenSSL file; web search
  worked (tried 2026-11-23, 26.1117.41037).
- **The temp folders denied**: other programs' files sit there. The
  cost: `git` prints a warning at each command (harmless). The one
  folder opened, `/private/tmp/codex-browser-use` — read, and its
  sockets allowed — is where the built-in browser's helper finds the
  browser; without it the browser is unavailable. → "The built-in
  browser".
- **`~/.config/gh-codex` read**: the token's folder (step 5), readable
  for `gh` and Git, kept from the agent's writes.
- **`:workspace_roots`**: the folder opened, with no path written.
  `.git` writable, for a pull and a commit; its `config` and `hooks`
  read-only, so that no hook or hooks path can be planted to run later
  outside the bounds; `.codex` and `.agents` read-only, so that the
  agent cannot change its own project settings or leave itself a skill
  — Codex reads a trusted project's `.codex/` and picks up a skill put
  under `.agents/skills/` by itself (the docs, read). These two are
  also the names Codex's own presets protect.
  **At the folder's top only** (step 4). A key in this table
  is read against whatever folder a chat is opened on, so a line added
  here — a vault's own among them — holds in every folder the user
  opens with Codex: a folder of the same name in another project is
  read-only there too (by how the keys are read, and seen for a chat's
  second root; another project not tried). → "`.git` is protected
  under a profile too".
- **`.claude` is not in the set: an optional line.** `".claude" =
  "read"` in this table — and `"<R>/.claude" = "read"` for a nested
  repository (step 4) — is for a user who opens the same folder with
  Claude Code as well. There, what Codex writes under `.claude/` (a
  settings file, a hook, a skill) is read by Claude Code, and a hook
  runs with the user's rights, outside any sandbox. Where Codex alone
  opens the folder, nothing reads `.claude/` (the docs name no such
  read). Codex's presets do not protect it; the line was this guide's
  own, on the test bed, and rested on no problem that arose. Its cost
  is the next point: most vaults built for Claude Code track files
  there. → *A tracked file under a read-only folder*.
- **A vault that tracks files under a read-only folder.** A path kept
  read-only is read-only to every program in the sandbox, Git among
  them: no line lets Git's write through and stops the agent's. So
  where the vault's repository tracks a file under `.codex/` or
  `.agents/` — or under `.claude/`, with the optional line — a pull
  that brings a change to it cannot write it, and does not fail
  cleanly: `git pull --ff-only` stops with an error, HEAD unmoved, the
  files under the read-only folder the old ones, **the other files of
  the same commits already rewritten** and shown as local changes; and
  every later pull refuses, in the user's own terminal too, until they
  are discarded. Nothing is silently reverted; the clone is stuck. A
  vault that tracks nothing under these folders never meets it. One
  that does has its routine pull in three steps (the vault's procedure
  says so, `vault-procedures.md` § 1):

  1. `git fetch` — inside the bounds, `.git` being writable;
  2. `git diff --name-only HEAD @{u} -- .codex .agents` (with the
     optional line, `.claude` too; for a nested repository, its own);
  3. nothing listed: `git merge --ff-only @{u}`. Something listed: the
     agent stops and says so, and the user runs `git merge --ff-only
     @{u}` in their own terminal, in the vault's folder — the commits
     are already fetched, so it needs no login there. A change to a
     tool's project files then enters the clone by the user's hand.

  If the half-written state is already there, the user, in their own
  terminal, with no work of their own uncommitted: `git stash
  --include-untracked`, `git merge --ff-only @{u}`, `git stash drop` —
  what the failed pull wrote is the incoming commits' own content.
  Tried with Git 2.54 on folders made read-only by file permissions —
  the failed pull, the three steps, the way back; **not under the
  profile**, where the refusal comes from the sandbox.
  → *A tracked file under a read-only folder*.
- **The network**: the hosts the routine reaches and no other, through
  Codex's proxy — here GitHub (`api.github.com` is for `gh`). A host
  not listed is refused before the command runs. Web search and the
  built-in browser are not filtered by it (the docs): they are a way out
  beside it. → "The network, GitHub alone"; "What it costs, and what it
  leaves".
- **`[shell_environment_policy.set]`**: only Codex's commands get the
  token's folder and the credential helper; the user's own terminal and
  `~/.gitconfig` are untouched. `GIT_CONFIG_GLOBAL` names a file that
  Git, in Codex's commands, reads as its global configuration in place
  of `~/.gitconfig`. The file takes `~/.gitconfig` in by its include —
  the name, the mail address and a global ignore file hold as before —
  and adds the helper for `https://github.com`; the empty helper first
  clears the macOS Keychain helper, which fails inside the sandbox.
  Tried inside the profile on 2026-11-23: Git read all of these, and
  `git ls-remote` on a private repository passed with a member's
  token.
  **Until 2026-11-23 the set gave the helper as pairs**
  (`GIT_CONFIG_COUNT = "2"`, `GIT_CONFIG_KEY_<n>`,
  `GIT_CONFIG_VALUE_<n>`): the test bed's form, with which `git
  ls-remote`, a pull and a push dry run passed on 2026-11-20. The app
  passes pairs of its own in those variables to the commands it runs
  (the setup agent's account on 2026-11-23, for ChatGPT's Sites; Git
  inside the profile listed the app's settings at scope `command`), so
  the set's pairs and the app's cannot both stand. Which side is
  replaced was not tried; the file leaves the app's alone.
  **The file is made before the profile is in force** — its folder is
  the token's (step 5), read-only to the agent afterwards: in the first
  chat, by the agent with an approval it explains first, or by the
  user's hand, as for the rules file (step 6). The command that stores
  the token later writes beside it and leaves it as it is (seen
  2026-11-23).
  **Its cost**: where the file is missing — the token's folder removed
  or renamed — Git says nothing and reads no `~/.gitconfig` either: a
  commit stops with "Author identity unknown", which points at the
  name and not at the file (tried with Git 2.54, outside the profile).
  → "A credential inside"; *A setup followed from its first step*, the
  third run.

**Writing it.** Under the profile the agent cannot write this file, and
it is not meant to: the file is its own bounds. So the user applies it.

1. The agent reads the user's present `config.toml` (the first chat can:
   under the presets the sandbox reads anywhere) and composes the whole
   merged file — the user's other keys kept, top-level keys before any
   table, no table twice.
2. The user makes a backup copy of the file: the way back. The file
   that `GIT_CONFIG_GLOBAL` names is made now, before the profile is in
   force (above).
3. The agent gives the whole file — in one block, or as a file in its
   working folder that the user opens and copies. The user opens
   `~/.codex/config.toml` in a plain-text editor — the folder is hidden:
   `open -e ~/.codex/config.toml` in a terminal opens it in TextEdit —
   selects all, pastes, saves: **the whole file, in one save**. An edit
   in two stages leaves a half-way file on the disk, and the running
   app reads it: with the two top lines in and the profile's tables not
   yet, the app showed `failed to load workspace requirements: invalid
   input parameter` (seen once, 2026-11-22; the cause not established).
   The setup chat, opened before the paste, reads the file back and
   compares it with what it gave.
4. **A profile put into a file that had none shows after the app is
   quit and started again**: its name appeared under the composer only
   after one restart (seen 2026-11-22, 26.1117.41037). Lines added to a
   profile already in force held from the next new chat, with no
   restart (2026-11-20, 26.1111.71704). Either way a chat already open
   keeps the permissions it started with, and work inside the bounds
   goes on in a new chat.

Tried: an edit of five lines pasted and saved by the user's hand with
the app running, in force in the next new chat, an open chat unchanged
(one version, some twenty-five minutes); the whole file applied by a
user's hand, in force after one restart (2026-11-22; how it was handed
over in that run was not recorded). Not tried: the read-back from the
open chat as a step of its own; the app writing the file on its own
while an editor holds it open, where the later save would win. → *A repository nested in the opened folder*, "Who writes
the lines".

## 4. When a repository is added inside the vault: its lines

**Not part of the first setup.** A user's clone of a vault starts with
no repository inside it; each user adds the ones they need, and the
vault's list grows. So the file of step 3 carries no such line. The
lines of one repository are written when the user adds it, before it
is cloned, by the vault's own procedure for adding one
(`vault-procedures.md` § 3) — a procedure apart from the setup.

The profile's protection of Git's files and the tools' folders reaches
the top of the opened folder only. For a git repository nested in the
vault, four lines under
`[permissions.workspace-fence.filesystem.":workspace_roots"]`, with
`<R>` its path from the vault's top:

```toml
"<R>/.git/config" = "read"
"<R>/.git/hooks" = "read"
"<R>/.codex" = "read"
"<R>/.agents" = "read"
```

A fifth, `"<R>/.claude" = "read"`, for the user who keeps `.claude`
read-only (step 3, the optional line).

- **Why**: these are the files another program reads and runs later,
  outside any sandbox, with the user's rights (`guide.md` § 2.15). In a
  nested repository with no lines all of them, and `.claude`, were
  written, with no approval asked (tried).
- **Why a line per repository, and no pattern**: a `read` pattern is
  rejected by Codex (read in its source); a `deny` pattern with `**`
  stopped the agent from removing or moving any folder (tried).
- **Before the clone.** A line may name a path that does not exist yet.
  A repository that exists before its lines is open until they are
  written, with no sign; so is one moved or renamed.
- **Who writes them.** Under the profile the agent cannot write this
  file. In a chat on the vault it prepares the four lines; the user
  makes a backup and pastes them under the table's last line (tried);
  they hold from the next new chat, with no restart (step 3, "Writing
  it"). Then the user clones the repository.
- **What they cost**: the agent cannot create a repository at a listed
  path — the clone is the user's — and cannot remove or move the folders
  that lead to it. If the agent pulls a nested repository that tracks
  files under its `.codex/` or `.agents/`, the pull meets what step 3
  describes.

Tried in two repositories of several, with all five lines. In the runs
of 2026-11-22 and 2026-11-23 this entry still had the lines of every
repository of a vault written at the first setup; that was wrong for a
vault whose users each add what they need. Not tried: the lines of one
repository added by a user led by a chat on the vault; a submodule or
a worktree. → *A repository nested in the opened folder*.

## 5. The token

Inside the sandbox the macOS Keychain is out of reach, so the user's own
`git` and `gh` login does not work there. What worked:

- **A fine-grained personal access token**, made by the user on GitHub
  for the vault's repository alone: Contents and Issues read and write,
  Metadata read; no Workflows, so a push that changes GitHub Actions
  files is refused. The permissions are what the vault's routine needs:
  a vault that keeps no issues needs no Issues.
- **Who can make it**: the repository's owner, or a member of the
  organization that owns it. At the token's creation the user selects
  that owner under "Resource owner", then the repository. A
  collaborator on a repository that another personal account owns is
  not offered the repository, and neither is an outside collaborator of
  an organization (GitHub's docs name this as a gap; seen with two
  accounts on 2026-11-21). The vault's procedure says who owns the
  repository. → `environments/github.md`, "Who can make a fine-grained
  token for a repository".
- **A link that fills the page in** (GitHub's docs, read 2026-11-22;
  not tried here):
  `https://github.com/settings/personal-access-tokens/new?name=<name>&target_name=<owner>&expires_in=<days>&contents=write&issues=write&metadata=read`
  opens the token's page with the name, the resource owner, the expiry
  and the permissions set. The repository cannot be set by a link: the
  user still chooses "Only select repositories" and the repository,
  then generates (→ `environments/github.md`, "A link that fills in
  the token's page").
- **In an organization, the token may wait for the owner.** By default
  an organization's owner approves each member's fine-grained token
  before it works, and a token made again waits again; an owner may
  turn that off (→ `environments/github.md`, "An organization").
- **Stored by the user, at once**, in their own terminal, never shown
  to the agent. GitHub shows the token's string once, on the page that
  follows its creation — also while it waits for approval — so it is
  stored before that page is left:

  ```
  GH_CONFIG_DIR=~/.config/gh-codex gh auth login --with-token --insecure-storage
  ```

  `gh` reads the token from its standard input. It is then plain text
  in that folder, which the profile makes read-only to the agent;
  `GH_CONFIG_DIR=~/.config/gh-codex gh auth status` shows whether it
  went in, the token masked. A paste that shows nothing invites a
  second paste, and the string is then wrong with no sign: let the user
  see that it went in, or check it (→ `environments/github.md`,
  "Handing the token to the command that stores it"). Seen 2026-11-22:
  a member's token, stored this way and approved by the owner, listed
  and read the repository inside the profile. Not recorded: whether
  the command took the token while it still waited for approval.
  `gh`'s own help advises `GH_TOKEN` rather than `--with-token` for a
  fine-grained token, whose narrow reach "may cause confusing
  behaviour" with other resources (read, gh 2.102.0); stored this way
  it worked for the routine on the test bed.
- **What it costs**: the agent can read the token. Its reach is that one
  repository and the network is GitHub alone; it can be revoked, and the
  user renews it at its expiry. In any other GitHub repository Codex's
  `git` offers this token and fails; if the agent is to pull or push a
  nested repository, the user adds that repository to the token — one
  token takes several repositories of the same owner, not of two.

**Where the user is neither the owner nor a member** — a collaborator
on a repository of another personal account — no such token can be
made. The ways then, each with what it opens; the user decides, and the
vault's procedure says which the vault offers:

| the way | what it opens, or costs |
|---|---|
| the vault's owner moves the repository to an organization and invites the users as its members | nothing more: each user then makes the token above. The work is the owner's, once (→ `environments/github.md`, "An organization", "A transfer of a repository to an organization") |
| a personal access token (classic), stored the same way | GitHub: "can access every repository that you can access". The profile still bounds the Mac's files; on GitHub the agent's reach is the user's whole account. Small for an account that reaches nothing else, large for one that holds other work |
| no credential inside | the agent commits; the pull, the push and the issues are the user's hand in their own terminal, at every session |

→ *A permission profile with the routine inside*, "A credential inside".

## 6. The forbidden rules (recommended)

`~/.codex/rules/<name>.rules`: what must never run, whoever approves. A
`forbidden` rule rejects the command before its process starts, under
the profile and under a preset (tried).

```
prefix_rule(
    pattern = ["git", "push", ["--force", "-f", "--force-with-lease", "--mirror", "--delete", "-d"]],
    decision = "forbidden",
    justification = "Rewrites or deletes shared history. Ask the user.",
)
```

and the same after `"origin"` and after `"origin", "main"`; and `git
commit` with `--no-verify` or `-n`, which skips a vault's pre-commit.

- **Why, inside a profile**: a forced push is still possible there — the
  sandbox bounds where a command writes and what it reaches, not what a
  permitted push does to the remote.
- **Its limit**: matching is by exact argument prefix. Each placement is
  a rule of its own and a form not named passes (`git push origin
  +main`; `guide.md` § 2.2). What must hold for the vault is kept at its
  remote.
- **Writing it**: a new file in a folder the profile closes to the
  agent. In the first chat, before the profile is in force, the agent
  writes it with an approval it explains first, or the user makes it by
  hand. Not tried in this form: on the test bed the file was written by
  a session of another tool.

→ *The ChatGPT desktop app on macOS*, "Command rules".

## 7. The check

In a new chat, opened after steps 3 to 6. The agent runs each and shows
the message; a refusal counts by its message (`guide.md` § 4 step 7).

In a chat that is not on the vault, before the harness's half ends —
the chat that set the file up keeps the lead, gives the user a short
text for the new chat and judges what comes back
(`vault-procedures.md` § 1). The token's two lines come once the token
is stored, in a vault's installation:

| what | the pass |
|---|---|
| the permission choice under the composer | shows the profile's name |
| the permission list Codex is given | the profile's entries, the denies marked not escalatable |
| a listing of a folder beside this one; a new file in the home folder; the same by a script (`python3 -c`) | each refused, "Operation not permitted" |
| the same again, asked to run outside the sandbox | refused, with no prompt |
| `curl https://example.com` | refused before it runs: the domain "is not on the allowlist" |
| `gh auth status` | reads the token from `~/.config/gh-codex`, masked |
| `git ls-remote` on the vault's repository | passes |
| `git push --force --dry-run origin main` | rejected by the rule, with its justification |
| a page in the built-in browser; a web search | both work (the browser may ask first, by its own setting) |

After step 8, in a new chat on the vault (the last two rows once a
repository has been added inside it, step 4):

| what | the pass |
|---|---|
| `git config <key> <value>`; a new file in `.git/hooks`; `mkdir .codex` | each refused |
| the vault's routine: its pull, its script, its issues through `gh`, `git push --dry-run` | each passes, with no approval asked |
| in a nested repository: `git status`; then `git config <key> <value>` and a new file in `.git/hooks` | runs; both refused |
| `find . -mindepth 2 -name .git -not -path './.git/*' -prune` | every repository it lists has its lines |

Anything not refused is said to the user, who decides. As tried, all of
these were run in chats on the vault; `git ls-remote` by the
repository's address from another folder, before the clone, passed on
2026-11-22 (Git printed a refusal to write a cache; the refs came all
the same).

## 8. The clone, and the record

- **The vault is cloned after the harness, by the user's own command**
  in their terminal. Before the harness, the agent that cloned it would
  be one with no bounds, using the user's credential. After it, the
  agent cannot: the profile keeps it from creating a repository's Git
  files (tried at a listed path).
- **Codex is then opened on the vault's folder.** When the folder is
  trusted, the app adds a `[projects."<path>"]` entry to `config.toml`
  by itself (seen). From there the vault's own first steps follow. A
  repository inside the vault comes later, when the user needs it: its
  lines first, then its clone by the user (step 4).
- **The record**: what was set, each value with its reason, and that the
  set was taken as proposed or what was changed — kept where the vault's
  procedure says, outside the vault.

## 9. Later: the entry for each occasion

A change is made by `guide.md` § 4, "Later": the smallest entry that
answers what arose, or none. Under the profile the agent can write none
of its own settings, so a change may be prepared in a chat on the vault
— it can read the vault's procedure there — and is applied by the user:
a backup, the paste, a new chat, the check.

| what arose | the entry | what else it opens |
|---|---|---|
| a repository is added inside the vault, moved or renamed | its lines (step 4), before the clone or the move | nothing |
| the vault begins to track a file under `.codex/` or `.agents/`, or a pull fails at one | no entry: the pull in three steps (step 3, "A vault that tracks files under a read-only folder") | — |
| the user begins to open the same folder with Claude Code | the optional `.claude` lines (steps 3 and 4) | nothing; a pull that changes a tracked file under `.claude/` then goes by the three steps |
| a pull fails at a file under `.claude/`, and Codex alone opens the folder | the optional `.claude` lines taken out | what Codex writes there is read by Claude Code, should it open the folder later |
| a program the routine needs cannot be read | a `read` line for its folder in `[permissions.workspace-fence.filesystem]` | that folder becomes readable to every chat |
| a host the routine needs is refused | a line in `network.domains` | what the agent has read can be sent there |
| the agent is to pull or push another repository | that repository added to the token on GitHub — no line | the token reaches it |
| the token expired | the user renews it, by the command of step 5 | — |
| a command must never run | a `forbidden` rule for each placement (step 6) | — |
| a tool reaches its helper through a Unix socket and fails | a `read` line and a `unix_sockets` allow for the socket's folder, as for the built-in browser | that folder |
| something must run outside the bounds | no entry: nothing leaves the profile; the user runs it in their own terminal | — |
| `git` warns of a cache it cannot write | no entry: harmless | — |
| a page has to be seen as it is now, and cached search does not show it | Web search: live (`web_search = "live"`; the Configuration screen) | the docs: cached search "reduces exposure to prompt injection from arbitrary live content" |
| the built-in browser asks at every page, and the prompts are no longer read (`guide.md` § 2.10) | the browser's browsing default: Always allow | a session can put what it has read into a URL and visit it unasked |
| the app was updated | no entry: the check of step 7 again, and a look at the composer | — |

A line is taken out the same way when its reason is gone.

## A lighter alternative: Approve for me, with no profile

For a user who does not want the token and the profile. **Not the set
proposed**: its weakness is that whether a command leaves the sandbox is
decided by the model that asks and the reviewer model that answers, for
any command, a write outside the folder included — and under it a chat
wrote into another repository on the Mac with no prompt to the user
(→ *An agent wrote outside its folder*).

- Step 2 as above.
- **The permission choice under the composer: Approve for me**, chosen
  in each chat. A vault's routine needs to leave the presets' sandbox at
  every pull, commit, push and `gh` call; under Ask for approval each is
  a prompt, and the reviewer answers them instead.
- **`config.toml`**, in place of step 3:

  ```toml
  sandbox_mode = "workspace-write"

  [sandbox_workspace_write]
  network_access = false
  ```

  No approval keys: the composer's choice supplies them. With the
  network off, a command that connects has to ask to leave the sandbox,
  which is reviewed.
- **No token**: outside the sandbox Codex uses the Mac's own `git` and
  `gh` login, signed in by the user before the first chat on the vault.
- **Step 6 as above**, and it matters more here: the forbidden rules are
  the only stop that no reviewer overrides.
- **A nested repository is open**: the presets protect Git's files and
  the tools' folders at the top of the opened folder only, and a preset
  has no place for a line (read in Codex's source; not tried under a
  preset).
- A line in `~/.codex/AGENTS.md` saying that when a vault's routine
  fails in the sandbox the agent asks to run it outside makes the
  routine steadier: in one chat the agent did not ask and reported a
  limit of its environment instead.

→ *The ChatGPT desktop app on macOS*.
