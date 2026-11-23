# Claude Code on macOS

What has been found here about Claude Code on macOS, in the desktop
app's Code tab, on one Mac, between 2026-11-01 and 2026-11-20 (Claude
Code v2.1.2xx to v2.1.3xx). Each point says whether it was **tried** on
that Mac or only **read** in the Claude Code docs (settings,
permissions, permission modes, sandboxing), with the date. One machine:
observations, not guarantees; where this note and the docs for the
user's version differ, the docs win. This note is the evidence: the set
proposed, in order and each value with its reason, is
`setup/claude-code-macos.md`, which points here by section; the text
that is applied is in `model/` (`settings.json`, `rules/harness.md`,
`user-memo.md`, `optional/`).

## What the standard does with no setting

Read in the docs, 2026-11-20; not tried with the settings removed.

- In the default mode (Manual) an action outside the working directory
  raises a permission prompt. That is the only mode in which the stop is
  deterministic.
- In Auto, the file tools (Read, Grep, Glob) read a path outside the
  working directory with no prompt and no classifier. A write outside —
  by a file tool or by a Bash command — is the classifier's call: a
  model decides.
- `.claude` is a protected directory: a write there prompts in Manual
  and Accept edits and goes to the classifier in Auto. Tried 2026-11-01:
  in Auto, a session opened on one folder edited the user's
  `~/.claude/CLAUDE.md` with the Edit tool; no prompt, no classifier
  block.

So the question for the user is which of these they accept. A user who
is content that a model judges an outside read or write needs none of
the options below. A user who wants "nothing outside the folder I
opened, unless I ask" held by something other than a model needs the
three that follow — each is a documented option, none is on by default.

## The three options

- **The read block**, `permissions.blockReadsOutsideWorkingDirectories:
  true` (v2.1.257+). Read, Grep and Glob refuse a path outside the
  session's folders with no prompt; a folder the user adds to the
  session is inside. A `true` in any settings file wins: there is no
  per-folder off switch under a user-level one (read). Tried: an outside
  Read refused — "blocks reads outside the working directories". Its
  cost: a Bash read of an outside path (`cat`, `ls`) prompts even in
  Auto; and before the sandbox was on, a command its parser could not
  trace (an inline `python3 -c`, a heredoc, a shell variable in a path)
  prompted even for a path inside (tried 2026-11-01, 2026-11-02).
- **The write block**, `permissions.blockWritesOutsideWorkingDirectories:
  true` (v2.1.314+). Write, Edit and NotebookEdit refuse a path outside
  the session's folders in every mode; no allow rule passes it (read).
  Tried 2026-11-09: an outside Write refused with no prompt. It does not
  bound Bash, MCP tools or hooks; the docs pair it with the sandbox. Its
  designed cost: Claude Code settings files are refused to the agent
  everywhere, inside the session's folders too — "Writes to Claude Code
  settings files are blocked while
  permissions.blockWritesOutsideWorkingDirectories is on, because a
  settings file can lift that block" (seen 2026-11-09, on a vault's
  `.claude/settings.json`). So the user applies every change to a
  settings file by hand — their own and a vault's project settings — and
  the agent reads it back.
- **The sandbox**, `sandbox.enabled: true`. Each Bash command runs under
  macOS Seatbelt: writes in the session's folders and the temp folder
  alone; with the read block on, reads there alone (plus what commands
  need: the global git configuration, parts of `~/.claude`); no host
  unless named on the command and approved (read). Tried 2026-11-09, in
  Auto: an outside `touch` — "Operation not permitted", no prompt; `git
  ls-remote` with no host named refused by the proxy. The docs call it
  "not a complete isolation boundary": the file tools, MCP servers and
  hooks run outside it.
  - **Commands that need a credential the sandbox hides fail inside
    it.** Tried 2026-11-09: `git push` over HTTPS — "could not read
    Username" — because git's credential comes through `gh`, whose
    folder sandboxed commands cannot read; `gh` itself likewise. The
    docs' remedy is `sandbox.excludedCommands`; the template excludes
    `git push *`, `git pull *`, `git fetch *` and `gh *`. Those then run
    with the user's rights, no OS bound. Opening gh's folder to the
    sandbox instead would hand every command the token.
  - **A call is excluded only when the entries cover every command in
    it** (read; tried 2026-11-09): `git -C <path> push`, a chain `git
    commit … && git push`, a pipe (`gh … | head`), a redirect, a
    substitution, or `gh` called from inside a script all stay sandboxed
    and fail. Hence the rules-file line that these commands go as a call
    of their own.
  - **`sandbox.failIfUnavailable: true`.** Unset, a sandbox that cannot
    start leaves commands running unsandboxed without a word (read). Set,
    Claude Code stops at startup, or refuses each sandboxed command
    (read). Tried 2026-11-20: with it set, sessions start and commands
    run. How the desktop app shows a sandbox that cannot start: not
    seen.
  - **With the sandbox on, the read block's prompt on an untraceable
    command is gone for a sandboxed command.** Tried 2026-11-20, in
    Auto: `python3 -c`, a heredoc fed to `python3 -`, a `for` loop, a
    subshell, a command substitution — none prompted (a shell variable
    in a path: not tried). So rules-file lines telling the agent to keep
    commands simple are no longer needed and left the template. An
    excluded command still gets that prompt: `gh … --jq` with brackets
    in the filter prompted in Auto (tried 2026-11-09). The docs say
    `autoAllowBashIfSandboxed` defaults to `false` since v2.1.324; what
    a sandboxed command meets in a mode other than Auto was not tried.

## Deny rules by command name

A `Bash(<name>:*)` deny stops the spelling it names: not the command
inside a string (`sh -c '…'`), a full path to the program, another
program for the same act (`python3 -m pip`), or a script's own calls.
The docs describe deny rules as a stop on the forms Claude usually
writes, not a boundary (read). Until 2026-11-20 the template denied
`sudo`, the package installers, `curl`, `wget`, `ssh`, `scp` and the
`claude` and `codex` CLIs by name. They were taken out: no problem had
ever been recorded from them, a line that stops one spelling gives a
false sense of a stop, and with the sandbox on what a command can write
and reach is bounded by the OS whatever its name. A `Bash` deny also
binds sandboxed commands and takes no exception (read), so a name
denied "to be safe" stops the wanted uses too.

What stays in the template: denies on **paths** (the secrets — matched
whatever program is used, for the file tools and recognised Bash file
commands; the sandbox hides the same folders from commands), on the few
commands whose job is to **print a secret**, and on desktop-app tools by
which a session widens its own scope. Where there is no sandbox (native
Windows) the ground for dropping the name denies does not hold.

**Another agent started from the shell** (`guide.md` § 2.7) was the
case the dropped `claude` and `codex` name denies had covered. Tried
2026-11-20 on that Mac, in Auto, with the sandbox and both blocks on
(the `codex` CLI was not installed there: that half was not tried):

- **Inside the sandbox, `claude -p …` did not start**: the OS refused
  the exec ("operation not permitted"). The binary sat under the home
  folder, which the read block closes to sandboxed commands — a fact of
  where it was installed, not a general property: a CLI installed
  elsewhere may start.
- **A started process stays inside.** `python3` starting `sh` starting
  `touch` on a path outside the folder: "Operation not permitted". The
  sandbox is inherited by a child and its child, so an agent that did
  start inside would write and reach only what the sandbox allows.
- **The retry outside the sandbox** that Claude Code offers a failed
  command, for `claude --version` and for `claude -p --permission-mode
  bypassPermissions …`: each raised the user's permission prompt, in
  Auto — under the read block the retry was not left to the classifier.
  `sandbox.allowUnsandboxedCommands: false` removes the retry altogether
  and leaves the excluded commands unsandboxed (read).
- **Not observed: an excluded command starting a program.** The excluded
  commands run outside the sandbox, and some forms of them start another
  program (a `gh` alias that begins `!`, a git option that names a
  command such as `--upload-pack=`). The trial of the `gh` form was
  stopped before the alias was set up, by a model's safety judgement
  that had seen the user ask for the test; the git form was not reached.
  A deny on a command name cannot close this route — the rule matches
  the call's text, not what a subprocess starts — and the exclusions are
  what lets `git push` reach its credential. What stands on it is a
  judgement, not a rule.

So with the sandbox and both blocks on, a deny line per agent name adds
little: it would close again a spelling the sandbox and the prompt
already close, and would not reach the one route left. Without the
sandbox none of this holds. The desktop app's own way of starting a
session is the ask rule below; Claude Code's own subagents inherit the
session's permission rules (read 2026-11-20) and are not this case.

## A repository nested in the opened folder

A vault may hold another git repository in a subfolder — its own `.git`,
ignored by the vault's. The files that matter there are the ones another
program reads and runs later, outside any sandbox, with the user's
rights: Git's `config` (it can name commands) and `hooks`, and a tool's
project settings and hooks (`.claude/`).

Tried 2026-11-20, in Auto, with the read block, the write block and the
sandbox on and no line about `.git` or a nested `.claude` — one
repository opened, another nested in it:

| target | sandboxed Bash, top | sandboxed Bash, nested | Write/Edit tool |
|---|---|---|---|
| `.git/hooks/<file>` | refused | refused | written, no prompt (top) |
| `.git/config` | refused | refused | written, top and nested |
| `.claude/settings.json` | on the sandbox's write-deny list | written | refused (nested) |
| `.claude/hooks` | on the sandbox's write-deny list | written | not tried |
| `.claude/skills/…` | refused | written | written, top and nested |

(The write-deny list is the one a session is shown for its own sandbox.
The file tools' writes went through the classifier, which had seen the
user ask for the test: not a deterministic result either way.) So the
sandbox keeps commands off Git's hooks and config at both depths — wider
than the docs' "in your working directory only" — but the file tools
were not bound there at all; and the sandbox's protection of `.claude`
stops at the opened folder's top. What is planted in `.git/hooks` or
`.git/config` runs at the next `git push` or `git pull`, an excluded
command, outside the sandbox.

The five lines that closed it, in the user settings (the template has
them):

```json
"permissions": { "deny": [
  "Edit(**/.git/hooks/**)",
  "Edit(**/.git/config)"
] },
"sandbox": { "filesystem": { "denyWrite": [
  "~/**/.claude/settings.json",
  "~/**/.claude/settings.local.json",
  "~/**/.claude/hooks/**"
] } }
```

Tried after they were in, 2026-11-20, in Auto, in the same session and
in a new one: an Edit of `.git/hooks/<file>` and of `.git/config`, at
the top and in the nested repository, and a Write of a new file in the
nested `.git/hooks/` — all refused with no prompt ("File is in a
directory that is denied by your permission settings"); sandboxed, in
the nested repository, `touch .claude/settings.json`, `touch
.claude/settings.local.json` and `mkdir .claude/hooks` — "Operation not
permitted"; a plain file in the nested `.claude/` still written. Nothing
wanted is lost: Git writes its own files, and the write block already
refuses settings files to the agent.

**These patterns hold at any depth, so nothing is added per
repository**: a repository cloned into the vault later is covered
without a new line. What they leave:

- **Skills** (`.claude/skills/`) in a nested repository: writable, by
  commands and by the file tools — an instruction file like any other in
  the folder.
- **Hook programs a vault keeps in ordinary folders** (a hooks folder
  named by `core.hooksPath`, a script a hook calls): writable like any
  file. Protecting them is the vault's matter.
- **A submodule or a worktree**, whose Git folder lies elsewhere (under
  the parent's `.git/modules/`): not tried; the two deny lines name
  `.git/hooks` and `.git/config` only.
- **A nested repository's push.** An excluded `git push` is a call of
  its own, so it acts on the repository of the session's working
  directory; `git -C` and a `cd` in the same call stay sandboxed and
  reach no host (read). A `cd` as a call of its own, then a bare `git
  push`, would reach the nested repository's remote: not tried. What
  stands against it is an instruction (the rules-file line on `cd`) and
  the classifier, not a setting; a repository an agent must never push
  is protected at its remote (branch protection).

## How rules and paths are read

Read 2026-11-20 unless marked.

- Rules are judged deny, then ask, then allow, before the permission
  mode: a deny binds in every mode, and binds subagents.
- `Edit(**/name/**)` matches at any depth below the working directory
  (tried, above); a pattern without the leading `**/` matches at the
  working directory alone. An `Edit` deny also binds the Bash file
  commands Claude Code recognises and is added to the sandbox's
  write-deny list.
- In the user settings a permission rule's path written `/…` is relative
  to `~/.claude/`: write `~/…` for the home folder or `//…` for an
  absolute path.
- A `sandbox.filesystem` path in the user settings is anchored at
  `~/.claude` unless it begins `~/` or `/`; `*`, `?` and `**` work in
  these lists on macOS (the docs' example: `~/**/.env`). Hence
  `~/**/.claude/…` above.
- A rule of the form `mcp__<tool>(args)` is ignored in settings files: a
  tool is denied or asked whole.
- **Ask rules hold in every mode** (read; tried 2026-11-02 in Auto: the
  prompt appeared at `mcp__ccd_session__start_session`). The template
  has two: `Edit(~/.claude/**)`, so the user's agent files change only
  on a yes, and the desktop app's tool by which a session starts another
  session. Whether an `Edit` ask rule also catches a Bash write (a
  redirect, `tee`): not tried.

## Other things seen

- **Added folders are inherited.** A folder the user adds to a session
  is open to its subagents (tried 2026-11-01) and to a session it starts
  (tried 2026-11-20: a session started from one that had `~/.claude`
  added could read it). A check meant to run without an added folder is
  opened by the user, fresh.
- **Looking at a web page.** A sandboxed `curl` needs its host named and
  approved for that command. Tried 2026-11-19, in Auto: two `curl`
  status checks raised permission prompts, and after the user's yes
  still failed — the yes ran the command but did not open the host. The
  WebFetch tool runs outside the sandbox and needed no prompt (it
  returns a model's rendering, not the bytes). One user's remedy was a
  rules-file line: a page that is only looked at is read with WebFetch;
  a command connects only when a file's own bytes are needed.
- **Tools that fail inside the sandbox.** `ps` ("operation not
  permitted"); a Unix socket (blocked by default —
  `sandbox.network.allowUnixSockets` takes a folder, tried 2026-11-11);
  localhost (`sandbox.network.allowLocalBinding`, which also allows
  listening); a program that writes a lock file in `/tmp` (the sandbox
  writes `$TMPDIR`). Each is opened when the need is met, by the entry
  that names it, not in advance.
- **Accept edits and an added folder.** With `~/.claude` added to a
  session, Accept edits passed a write there with no prompt (seen
  2026-11-01, before any ask rule on that folder).
- **Backups of settings files** under `~/.claude/` cannot be made by a
  sandboxed `cp` (most of `~/.claude` is write-denied to commands): made
  with the Read and Write tools, in a session to which the user has
  added `~/.claude`, under the ask rule's prompt (tried 2026-11-09).
- **"Yes, and don't ask again"** in a prompt saves the approved command
  as an allow line in the folder's `.claude/settings.local.json`,
  verbatim — a secret typed in the command included (reported publicly,
  2026-11; the file is git-ignored by default). Read that file from
  time to time.

## Open reports

Read 2026-11-23 in the tool's issue tracker
(`github.com/anthropics/claude-code`), the newest version then being
v2.1.330; none was tried here, and none carried a maintainer's reply.

- **An excluded command still runs sandboxed** (#89931, opened
  2026-08-26, open, marked a regression 2026-11-20; #95813, open): on
  macOS, with `sandbox.excludedCommands` naming `git` and `gh`, `gh`
  failed on TLS and a push over SSH broke, as inside the sandbox;
  reported from v2.1.232 to v2.1.294. Not seen on the Mac of this
  note, where the four exclusions carried a push and a pull daily. The
  reports' own way round was the retry outside the sandbox, which
  raises a prompt.
- **The read block prompts for some `gh` calls in Auto** (#91479,
  open; v2.1.258 to v2.1.312): a `--jq` filter — "names a path that is
  computed at run time". Seen here too ("The three options").
- **The read block does not count folders added in a project's or a
  local settings file** (#92582, open): a folder the user adds to the
  session, or names in the user settings, is counted.

## Not known here

Whether a program started through an excluded command (a `gh` alias, a
git option that names a command) runs unprompted; an agent CLI installed
outside the home folder, started from a sandboxed command; the `codex`
CLI started from Claude Code's shell; how the desktop app shows a sandbox that
cannot start; what a sandboxed
command meets in Manual or Accept edits since `autoAllowBashIfSandboxed`
defaults to `false`; a submodule's or a worktree's Git folder under the
two deny lines; a `cd` followed by a bare `git push` in a nested
repository; whether an `Edit` ask rule catches a Bash write; the file
tools on a nested `.claude/hooks`; why some `gh` forms prompt (the read
block's parser and brackets — the exact set is undocumented).
