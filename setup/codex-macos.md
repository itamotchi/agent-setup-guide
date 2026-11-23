# Codex in the ChatGPT desktop app on macOS: the setup

A worked route: how Codex in the ChatGPT desktop app on macOS is used
on a vault with nothing set — the app as it starts, on its own
permission choice "Ask for approval" — tried on one Mac on 2026-11-23.
A reference for the user's agent, which builds the harness itself by
`guide.md` § 4: read this first, check each point against the app as
it is and its present docs — the app changes within weeks — take what
still holds, and say what does not. It is not a procedure to apply
unread, and a vault's procedure is not made of it
(`vault-procedures.md`).

Three files work together. `guide.md` holds the procedure, for any
tool. This file records the route for this tool and why. The dated
findings and trials it rests on are in `environments/codex.md`, named
here by section as "→ *section*". A reference, never a requirement:
the harness is the user's (`guide.md` § 3), and where this file and
the docs for the user's version differ, the docs win.

**Until 2026-11-23 this file gave another route**: a permission
profile in `~/.codex/config.toml` that also bounded reads, with a
token of its own for the agent. It is no longer the route — why is
under "Other forms" at the end; what was found under it stays in
`environments/codex.md`.

## What it is, and how far it was tried

**The form.** Nothing is set. A newly installed app starts every Codex
chat on the permission choice **Ask for approval**, and that choice is
kept. No file of bounds is written, no token is made for the agent,
no line is added for a vault.

**What the user gets.** Codex's own sandbox, which the operating
system enforces: a command the agent runs writes inside the folder the
chat is opened on (and the temp places) and nowhere else, and has no
network. Whatever has to leave that sandbox — a write anywhere else, a
connection, a write to Git's own files — comes to the user as a
prompt, each time; refused, it does not run. So nothing is written
outside the opened folder unless the user has said yes to that very
command.

**What it does not give** — said to the user before they begin (the
whole of it under "What to watch"):

- reads are not bounded: the agent can read whatever the user can, on
  the whole Mac;
- a command the user allows runs outside every bound, with the user's
  own rights and the user's own GitHub login;
- the stop is the user: a prompt allowed without being read stops
  nothing.

**Tried**: one Mac, the ChatGPT desktop app 26.1117.41037, 2026-11-23,
a Business workspace, in the just-installed state (the user's
`~/.codex` set aside first), in chats opened on a throwaway repository
and on a clone of a vault whose instructions have the session pull at
its start, build an index by a script, read open issues with `gh`,
commit and push. → *The app as it starts: Ask for approval*.

| what the agent did | what happened |
|---|---|
| a write inside the opened folder | passed, unasked |
| the vault's own script | ran, unasked |
| `git pull --ff-only` | one prompt; passed on the user's yes |
| `gh api` (the open issues) | one prompt; passed |
| `git add` and `git commit` | one prompt for the pair; passed |
| `git push --dry-run` | one prompt; passed |
| a write outside the folder | one prompt; refused, and not made (looked for by the user) |
| a second and a third message in the same chat | answered; the folder kept |

A session with a start, one commit and one push was four prompts.

**Not tried**: a real push (a dry run passed); the prompt's "allow
similar commands" (below, step 3); a repository nested in the opened
folder under this form; the General switch for Full access turned off
in this state; a user who starts with no GitHub login on the Mac; the
CLI, another platform, another version of the app. The app's open
reports for this very version describe prompts that never appear and
commits that fail ("What to watch", 7); they did not show here.

**How it was chosen.** Among what users of the app are found doing,
not derived from the docs. The vendor's page on the permission choices
says "For most work, start with Ask for approval"
(`learn.chatgpt.com/codex/permission-modes`, read 2026-11-23). Public
accounts of the app, read through a research summary the same day and
not checked here: the guides for newcomers give Ask for approval as
the start; the accounts of use over time that were found are of
Approve for me; no account was found of a profile that bounds reads
run over time — only fault reports — nor of the app on a shared
knowledge base with a Git routine. Of the two choices in use, Ask for
approval is the one under which leaving the folder is never a model's
decision. → *What users of the app are found doing*.

## 1. The app's own settings

The agent cannot see these screens: it names a thing and says what it
is for; the user looks. Names move between versions and languages;
the user's agent goes by what the user's screen shows.

**Needed — one look, and nothing changed.**

- **The permission choice under the composer shows Ask for approval.**
  In a newly installed app it does, in a new chat and in a new project
  alike (seen 2026-11-23; in the Japanese interface 「承認を依頼」 under
  the composer, 「承認を求める」 in its menu). The menu also offers
  Approve for me and Full access. The choice is the user's, chat by
  chat; nothing keeps it but the user.
- **What the other two mean**, said once:
  - *Approve for me*: the same sandbox, but a request to leave it is
    answered by a reviewer model in the user's place, and no prompt
    reaches the user. Fewer prompts; whether something is written
    outside the folder is then a model's decision. Under it, on one
    Mac, a chat rewrote files in another repository with no prompt to
    the user (→ *An agent wrote outside its folder*). A user may choose
    it, knowing that.
  - *Full access*: no sandbox and no approvals.

**Recommended** — none is needed for the form to hold:

| switch | why |
|---|---|
| General, Full access: off | With the switch off that choice is absent from the menu under the composer (seen 2026-11-19), so it cannot be picked by a slip. In a newly installed app the switch was on (seen 2026-11-22). |
| Codex memories off (the docs: off by default) | The docs: "a helpful recall layer, not ... the only source for rules that must always apply". |
| The "Codex" custom-instructions field empty | Undocumented, and it also reaches Codex Cloud; the documented place for the user's own instructions is `~/.codex/AGENTS.md`. |
| Notifications for permission requests and questions on | A prompt that reaches the user is seen. |
| Plugins the work does not use off | One thing less that can act. |
| Browser: history access off, site tools (WebMCP) off, downloads and uploads asked | The docs: history "can include internal URLs, search terms, and other sensitive information". |
| Git: always force push off | It rewrites a shared vault's history. |
| Import not run | Another tool's settings and instructions are that tool's. |

**Outside the sandbox — one sentence to the user.** The sandbox bounds
the commands the agent runs. It does not bound what acts as the user
elsewhere: computer use, the Chrome extension, passwords saved in the
app, control of the Mac from another device, a GitHub connection in
ChatGPT, cloud tasks. Each takes an act of the user's to begin. The
advice is to leave them off while the vault is in use.

→ *The ChatGPT desktop app on macOS*, "Other settings that bear on
Codex".

## 2. Opening Codex on a folder

- **A new project, with the folder as its source folder.** In the app
  an opened folder is a project: the user makes a new project and
  chooses the folder as its "source folder" (seen 2026-11-23; in the
  Japanese interface 「ソースフォルダ」). "Open the agent on that
  folder" is not an act a newcomer knows how to do in this app: say it
  in these words.
- **A chat with no folder chosen** is given a working folder by the
  app, under `~/Documents/Codex/` (seen 2026-11-22). It will do for a
  session that sets nothing up in a vault.
- **When the folder is trusted** the app adds a `[projects."<path>"]`
  entry to `~/.codex/config.toml` by itself (seen 2026-11-19).
- **A vault is cloned by the user's own command** in their terminal,
  with their own login, then opened so. Under this form nothing has to
  be set before the clone.

## 3. The prompts: what the user answers

This is where the form lives. The user is told what will come and how
to answer it, before the first chat on the vault.

- **What a prompt looks like** (seen 2026-11-23): a question in a
  sentence, the command under it, and two buttons — refuse (「拒否」)
  and allow once (「一度だけ許可」). The sentence is the agent's own
  wording: it names the vault's step ("as the vault's start …") and
  differs at each request. **The command under it is what will run.**
- **What a vault's routine raises**: one prompt at each act that
  leaves the sandbox — the pull, the reading of issues, the commit,
  the push (the table above). They come because, inside the sandbox,
  Git's own folder is read-only, there is no network, and the Mac's
  stored login is out of reach.
- **How to answer** — the vault's procedure can name its routine's
  acts, so that the user knows them when they come:
  - the routine's acts, when the user has asked for the work: allow
    once;
  - a write outside the opened folder, or anything the user did not
    ask for: refuse;
  - a prompt the user does not understand: refuse, and ask the agent
    what it was for. A refusal is the safe side: the command does not
    run, and nothing is lost.
- **"Allow similar commands."** A small arrow beside "allow once"
  opens a second choice (「類似コマンドを許可」): the app then allows,
  for the rest of that conversation, every command that begins the
  same way. The beginning is the app's choice and may be wider than it
  looks: for the reading of issues it offered `gh api`, which also
  covers a write through GitHub's API. Not tried here. A user tired of
  the prompts looks at what beginning is offered before taking it.
- **Prompts wear attention down** (`guide.md` § 2.10). Four in a
  session were read; if a vault's routine raises many more, the
  routine is what to look at — fewer, larger acts — before the choice
  is moved to Approve for me.

## 4. Git and GitHub

- **No token is made for the agent.** An allowed command runs outside
  the sandbox with the Mac's own `git` and `gh` login — the one the
  user signed in with in their own terminal (`gh auth login`), kept in
  the macOS Keychain. Seen 2026-11-23: outside the sandbox `gh auth
  status` showed the user's account from the keyring, and the pull and
  the push dry run passed with Git's own Keychain helper.
- **Inside the sandbox the login is out of reach**: there `gh auth
  status` says the token is invalid, and Git cannot resolve the host.
  That is the sandbox, not a broken login; the agent then asks to run
  the command outside.
- **What it means**: on GitHub an allowed command reaches whatever the
  user's login reaches — every repository of the account, not the
  vault's alone. What must hold for the vault is kept at its remote
  (branch protection, no force push), not on the user's machine
  (`guide.md` § 2.14).
- **The user's own hands**: installing `git` and `gh` and signing in,
  in their own terminal, one command at a time, the agent giving each.

→ *The app as it starts: Ask for approval*, "The login".

## 5. What to watch

Said to the user plainly; none of it is closed by a setting on this
route.

1. **Reads are open.** The agent reads whatever the user can read on
   the Mac — other projects, other vaults, files of secrets. Codex's
   settled part has no bound on reads; the one feature that has is
   marked Beta by the vendor and gave way on this version ("Other
   forms"). A secret is kept where the user would not want any program
   to read it.
2. **An allowed command is outside every bound.** It writes anywhere
   and connects anywhere, with the user's rights. The prompt is the
   only stop, and it stops only when read.
3. **The choice under the composer.** On Approve for me or Full access
   the stop is no longer the user (step 1). A model chosen in the app
   may move the choice by itself (the docs, read 2026-11-19): look at
   it when a chat starts.
4. **A commit runs the vault's Git hooks outside the sandbox.** The
   hooks' files sit in the opened folder, where the agent writes
   unasked; the prompt shows the commit, not the hook's code
   (`guide.md` § 2.14).
5. **A repository nested in the opened folder is not protected.**
   Codex keeps Git's folder and its own project folders (`.codex`,
   `.agents`) read-only at the top of the opened folder only (the
   source, read; seen under a profile, not tried under this form), and
   this form has no place for a line: in a nested repository Git's
   `config` and `hooks` can be written with no prompt, and what is
   planted there runs later with the user's rights (`guide.md`
   § 2.15). Say so to a user who adds one.
6. **Looking at the web is beside the sandbox.** Web search and the
   built-in browser are not bounded by it (the docs); with the
   browser's browsing default on "always allow", a session can put
   what it has read into a URL and visit it unasked.
7. **The app changes, and this version has open reports.** In the
   tool's issue tracker (`openai/codex`), open on 2026-11-23 and read
   through a research summary, not checked here: under Ask for
   approval, requests to leave the sandbox rejected before any prompt
   (#32563); a commit failing on Git's lock file with no prompt
   offered (#63187, this version; #61936); the choice made under the
   composer not reaching a chat (#63033, #62975, this version). None
   showed on the Mac of the trial. After an update of the app, the
   check of step 6 is run again.
8. **`~/.codex/config.toml` is left alone.** A permission set written
   there shows as a fourth choice, "Custom", and the older keys and a
   profile do not combine (the docs). On this route the file holds
   only what the app writes.

## 6. The check

Nothing was set, so there is no file to compare: the check is of the
app as it is on the user's Mac — at the first setup, and again after
an update. In a chat opened on a folder under version control, the
agent runs each and shows the output; the user looks at the screen.

| what | the pass |
|---|---|
| the permission choice under the composer | shows Ask for approval |
| a new file inside the folder | made, with no prompt |
| a new file beside the folder (outside it) | a prompt comes to the user; refused, the agent reports the refusal, and the user finds no such file |
| `git add` and `git commit` of the file inside | a prompt; allowed once, the commit is made |

A prompt that does not come where one should — the file outside made
unasked, or the commit refused with nothing shown — is said to the
user, and the vault is not opened until it is understood: it is the
kind of fault the open reports describe ("What to watch", 7).

## 7. Later: the entry for each occasion

A change is made by `guide.md` § 4, "Later": the smallest entry that
answers what arose, or none.

| what arose | the entry | what else it opens |
|---|---|---|
| the prompts of the routine are no longer read | first the routine itself (fewer acts that leave the sandbox); then "allow similar commands" for a beginning the user has looked at (step 3); only then Approve for me | under Approve for me, leaving the folder is a model's decision |
| a command must never run, whoever allows it | a `forbidden` rule in `~/.codex/rules/<name>.rules`, one for each placement of the argument: it rejects the command before its process starts (tried under this sandbox, 2026-11-19; the docs mark rules experimental) | — |
| a repository is added inside the vault | no entry exists on this route; "What to watch", 5, is said | — |
| the user begins to open the same folder with another agent tool | no entry; that tool's own harness is its own | — |
| a page has to be seen as it is now | Web search: live (the Configuration screen) | the docs: cached search "reduces exposure to prompt injection from arbitrary live content" |
| the app was updated | no entry: the check of step 6 again, and a look at the composer | — |

→ *The ChatGPT desktop app on macOS*, "Command rules".

## Other forms, and why they are not the route

- **Approve for me.** The form in which most accounts of continued use
  were found. Not the route here because whether a command leaves the
  folder is then decided by the model that asks and the model that
  answers (step 1; → *An agent wrote outside its folder*). A user who
  takes it does so knowing that.
- **A permission profile that also bounds reads** (`:root = "deny"`
  with the opened folder written in, the routine's network through a
  proxy, a token of its own for the agent): this file's route from
  2026-11-20 to 2026-11-23. It held what this route leaves open —
  reads, and a way out that no model could open — and it was set up
  from its first step four times. It is not the route because the
  permission profile is marked Beta by the vendor and the proxy
  experimental; because on the app 26.1117.41037 a chat opened on a
  repository under it lost the opened folder at its second message
  and, where the folder held an instruction file, stopped there
  (seen); and because no account was found of anyone running such a
  profile over time. Its text is in this repository's history (this
  file before 2026-11-23). → *A permission profile with the routine
  inside*; *A profile that bounds reads gave way*.
- **No way out at all** (`approval_policy = "never"`): the sandbox
  then holds whole, and the agent cannot pull, commit or push — Git is
  the user's hand in their own terminal. Not tried here.
- **A container or a virtual machine** with the folder alone inside:
  where reads must stay in one folder, the form its users report. Not
  tried here, and more than a newcomer sets up.
