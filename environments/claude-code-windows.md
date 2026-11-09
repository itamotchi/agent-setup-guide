# Claude Code on native Windows

What has been found here, as of 2026-11-09 (Claude Code v2.1.31x), from
the official docs read by WebFetch; nothing below has been run on a
Windows machine yet. Templates: `model/windows/` (`settings.json`,
`rules/harness.md`).

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
  the macOS one with `brew` and `security` out and `winget` in.
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

## Checks on Windows

The check of `guide.md` § 4 step 6, for this platform. A Bash write
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
