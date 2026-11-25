# Claude Code on native Windows

What has been found here, as of 2026-11-09 (Claude Code v2.1.31x), from
the official docs read by WebFetch. Since 2026-11-25 there is one run
on a Windows machine ("Run on a machine", below); it tried a vault's
hooks, Git and `gh`, and not the template: the template `model/windows/`
(`settings.json`, `rules/harness.md`) and the checks at the end of
this note are still untried.

Native Windows with Git for Windows (Git Bash), the desktop app assumed;
a user of the CLI extends this note. `~/.claude` is
`%USERPROFILE%\.claude` (the settings page).

- **No sandbox.** It runs on macOS, Linux and WSL2; "On native Windows,
  Claude Code runs commands unsandboxed" (the sandboxing page), with no
  warning when `sandbox.enabled` is set, and `failIfUnavailable` would
  stop Claude Code at startup. The template has no `sandbox` key. Say
  plainly what it costs: a Bash command is bounded by the deny list, the
  read block's prompt on an outside or untraceable read, the classifier
  in Auto and the instructions — not by the OS; a Bash write outside the
  session's folders, or another form of a denied command, meets nothing
  deterministic. The template's rules line on writes holds Bash, and the
  macOS line on the sandbox's excluded commands is dropped. The docs' way
  to the sandbox on Windows is Claude Code inside WSL2 — heavier; for a
  user who wants the OS to bound commands.
- **Why this template is not the macOS one.** On 2026-11-20 the macOS
  template lost its deny lines on command names and its rules-file lines
  on simple commands, on one ground: the sandbox bounds what any command
  writes and reaches, and takes away the read block's prompt on a
  command it cannot trace (`claude-code-macos.md`). With no sandbox
  neither holds, so the Windows template keeps those lines — a stop on
  the usual spelling of a command and no more, and that is to be said to
  the user. Two lines were taken over: `Edit(**/.git/hooks/**)` and
  `Edit(**/.git/config)`, which keep the file tools off Git's hooks and
  config at any depth; a Bash command can still write there on Windows.
  Not tried on Windows, like the rest of the template. Nothing here
  carries the macOS sandbox's protection of a nested `.claude`.
- **The PowerShell tool.** With Git Bash it is on by default for
  claude.ai and Console accounts, and a Bash rule does not restrict it; a
  Bash deny rule turns it off for the session without a warning (the
  tools reference). The template turns it off explicitly, `"env":
  {"CLAUDE_CODE_USE_POWERSHELL_TOOL": "0"}`, denies `powershell`, `pwsh`
  and `cmd` from Bash, and has a rules line to match. Its own rule form
  `PowerShell(...)` exists; its syntax was not read. Git Bash is needed —
  the Bash tool and Monitor run on it; without it PowerShell is the shell
  tool and the template does not fit (the setup page);
  `CLAUDE_CODE_GIT_BASH_PATH` in the settings' `env` if Claude Code does
  not find it.
- **Secrets.** Claude Code's own login is the file
  `%USERPROFILE%\.claude\.credentials.json` on Windows (the
  authentication page; on the Mac it is the Keychain). gh keeps its
  configuration in `%APPDATA%\GitHub CLI` (`gh help environment`). curl
  and git also read `_netrc` beside `.netrc`. The template's deny list is
  the macOS one as it stood before 2026-11-20, with `brew` and
  `security` out and `winget` in.
- **Paths in rules.** The docs give the POSIX forms only (`~/`, `//`,
  `/`) and say nothing of how `~` expands on Windows, of drive letters,
  backslashes, case, or a space in a path. The template writes them as
  on the Mac; the first check below tries one, and a line that does not
  match is fixed before anything else.
- **The read and write blocks** (`blockReadsOutsideWorkingDirectories`,
  v2.1.257+; `blockWritesOutsideWorkingDirectories`, v2.1.314+): no
  platform limit stated (the settings reference).
- **The desktop app** lists the same five modes with no platform
  qualifier; its Windows notes are on PATH, SSH and computer use (the
  desktop page). Auto: "All plans" (the permission modes page).
- **Backups and pastes.** A copy of each settings file before a change is
  made as on the Mac, in Git Bash; with no sandbox only the mode's prompt
  meets the command. A
  settings file is pasted thus: Windows key + R, `notepad
  %USERPROFILE%\.claude\settings.json`, Enter; Ctrl+A, Ctrl+V; check that
  Notepad's status bar says UTF-8; Ctrl+S. Then read back.
- **Local settings** load from the opened folder's own
  `.claude/settings.local.json` on Windows, not the repository root's
  (the settings page).

## Run on a machine (2026-11-25)

A vault's two sheets (`vault-procedures.md` § 1) were run by a user to
their ends on Windows 11 Pro: Claude Code's desktop app (its version
not recorded), Git for Windows with Git Bash, Python 3.15.2, gh
2.105.0 — the first sheet's harness, then the clone, the vault's
hooks, a new writer's first commit and push. The source is the log
that the session on that machine wrote in the vault, and what the user
told a session that supported him from elsewhere; no one but the user
saw the Windows screen. **What the first sheet's agent set there — its
settings file, which of the template's lines it took, the result of
its check of refusals, its record — was seen by no one but the user.**

Seen:

- **A vault's hooks ran.** Wired in the vault's `.claude/settings.json`
  as `python3 …` commands, under Git Bash: a write the vault forbids
  was refused by the PreToolUse hook's own message, made with the
  Write tool and with a shell redirect alike; at a Read, the
  PostToolUse hook's output reached the session. Not tried:
  `onFailure` (the hooks started).
- **Japanese through `gh api`.** A title and a body passed as `-F
  body=@<file>` arrived intact in an Issue. Not tried: the text given
  on the command line (`-f body='…'`).
- **Git.** A clone made with `-c core.autocrlf=false`; a pre-commit
  hook behind `core.hooksPath` (a Python program); three commits and
  pushes, with the user's own `gh auth login` — no credential made
  for the agent.
- **A session opened on an empty folder did not have the root
  `CLAUDE.md` after the clone**; one opened anew on the folder did.
  The docs: "CLAUDE.md and CLAUDE.local.md files in the directory
  hierarchy above the working directory are loaded at launch"; that
  one which appears later is picked up is not stated, the one re-read
  described being after `/compact` (the memory page, read 2026-11-25).
  Of settings the docs say otherwise: "Claude Code watches your
  settings files and reloads them when they change", a project's
  `.claude/` made during the session included (the settings page),
  with hooks held back "until you accept the workspace trust dialog
  for the folder" (the hooks reference). Whether the vault's hooks
  would have run in the first session was not tried.
- **A sub-folder's `AGENTS.md`.** In the session started on the empty
  folder, a Read of a file under a sub-folder brought that folder's
  `AGENTS.md` with it; in the session started on the clone, with the
  root `CLAUDE.md` loaded, it did not (seen the same on macOS that
  day). The docs: with a `CLAUDE.md` in the working directory or
  above it, "Your `CLAUDE.md` files only", under the default value
  `claude-md-or-agents-md` of "Project instructions" (the memory
  page). A vault that holds both files has its root file tell the
  session which folder files to open.
- **Local or cloud.** The desktop app starts both from one place:
  "use the environment selector, the first control in the row, to
  choose where Claude runs" — Local, "runs on your machine", or Cloud
  (the desktop page, read 2026-11-25). The user asked, before
  beginning, which was meant. Whether a Local session starts with no
  folder chosen is not stated.

## Checks on Windows

The check of `guide.md` § 4 step 7, for this platform. A Bash write
outside the session's folders (`touch`) would succeed here and prove
nothing; these instead:

- `cat ~/.claude/.credentials.json` in Bash: refused by the deny rule
  without a prompt is the pass. A prompt (the read block's) means the
  `~/` line does not match: refuse it, and fix the rules' form first.
- `powershell -Command "echo x"` refused; `curl --version` refused; `gh
  auth status` refused; asked, the session names no PowerShell tool
  among its tools.
- The rules' id command prints an id (`openssl` in Git Bash: not
  verified).
- An outside Read and a Write of a new file outside the session's folders
  refused without a prompt; `start_session` with `cwd` elsewhere raising
  a prompt; the memo and the rules loaded.
