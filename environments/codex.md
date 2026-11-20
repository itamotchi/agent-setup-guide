# Codex

What has been found here about Codex, dated and sourced: the docs as
read on 2026-11-09 and 2026-11-20, a vault's setup on Windows
(2026-11-09), trials in the ChatGPT desktop app on macOS (2026-11-19,
2026-11-20); and two reference configurations drawn from those trials,
each line with its reason. What was tried and what was only read — in
the docs or in Codex's source — is marked. **Codex's own protection of
Git's files and of its project settings, and the profile of the second
reference, hold at the top of the opened folder only**; a repository
nested below it needs lines of its own ("A repository nested in the
opened folder"). A reference, never a requirement: which
settings and guardrails a user has is theirs to decide with their agent
(`guide.md`). Where a note and the docs differ, the docs win; Codex
changes with each version.

## From the Codex docs (read 2026-11-09)

The pages under `developers.openai.com/codex/` redirect to
`learn.chatgpt.com/docs/`. No page shows a date; the Windows page cites
Codex CLI 0.162.0.

- **Native Windows.** The desktop app, the CLI and the IDE extension run
  natively; commands run in PowerShell under a Windows sandbox. WSL2 is
  the alternative for Linux tooling (the Windows sandbox page).
- **The Windows sandbox** (`windows.sandbox`):
  - `mxc` — "the recommended sandbox on compatible Windows devices"
    (Windows 11 24H2, build 26100.9278, or 25H2, build 26200.9278, and
    later): no administrator setup, no extra accounts, no firewall
    rules. The desktop app prefers it for consumer accounts when the
    device supports it; the CLI uses it with `[features] prefer_mxc =
    true`.
  - `elevated` — the preferred legacy fallback, and Codex's choice when
    no mode is set: its setup raises an administrator (UAC) prompt, which
    can be declined, and creates lower-privilege sandbox users, firewall
    rules and local policy changes. A change to the machine: the user
    decides it, told what it does.
  - `unelevated` — a restricted token and file permissions; network
    isolation weaker than `elevated`; no denied read paths.
  - Deny-read globs are evaluated when each process starts. Windows 10
    is "best effort".
- **Sandbox and approvals** (the agent approvals and security page).
  Sandbox modes `read-only`, `workspace-write`, `danger-full-access` (no
  sandbox, no approvals; "not recommended"); approval policies
  `on-request`, `never`, `granular`. A folder under version control
  starts in Auto (`workspace-write`, `on-request`: edits in the folder
  without asking, asks before editing outside it or using the network);
  a folder not under version control — this guide downloaded as a ZIP —
  starts `read-only`, and Codex may start `read-only` until the folder is
  trusted (an onboarding prompt or `/permissions`). Network access is off
  by default. Inside `workspace-write`, `<writable_root>/.git` is
  read-only, and `.codex`, `.agents` and `.aws` when they exist,
  recursively: a `git add` or `git commit` needs an escalation. The
  path is anchored at the writable root: a `.git` or `.codex` in a
  subfolder is not covered (Codex's source, read 2026-11-20; seen the
  same day, below).
- **Subagents** (read 2026-11-20). A subagent inherits the parent
  turn's sandbox and approval settings, and a custom agent file does not
  override them (a summary of the docs, not their words). A local Codex
  delegates only when asked, or when an `AGENTS.md` or a skill names the
  situation; the cost is tokens. So whether subagents are used is a way
  of working, not a matter of permission — and a user who wants Codex to
  use them as needed says so in `~/.codex/AGENTS.md`. Another agent
  started as a program from the shell is a different case (`guide.md`
  § 2.7): not tried here.
- **Who answers approvals** (the same page). By default the user
  (`approvals_reviewer = "user"`); with `approvals_reviewer =
  "auto_review"`, "eligible approval requests are reviewed by Auto-review
  instead of surfacing to the user" — sandbox escalations, network
  requests, app and MCP tool calls. It needs `on-request` or granular
  approvals; failures and timeouts block.
- **Command rules** (the rules page; "experimental"). `.rules` files in
  `rules/` beside each config layer (`~/.codex/rules/default.rules`; a
  repository's `.codex/rules/` only when trusted). `prefix_rule(pattern =
  [...], decision = ...)`: `allow` — "Run the command outside the sandbox
  without prompting"; `prompt`; `forbidden`; the strictest match wins.
  Matching is by exact argument prefix; a plain chain inside `bash -lc`
  is split, anything more is matched whole; PowerShell is not mentioned.
  A command added to the allow list in the TUI is written to the user's
  `default.rules`, and with Smart approvals (the default) Codex may
  propose a rule during an escalation. `codex execpolicy check --rules
  <file> -- <command>` shows the decision.
- **Settings.** The user's `config.toml` is in `CODEX_HOME` (`~/.codex`;
  on Windows, for example, `C:\Users\<name>\.codex`); a repository may
  carry `.codex/config.toml`; `trust_level = "untrusted"` for a project
  disables its project-local configuration.
- **Instructions.** A global `AGENTS.md` in Codex's home, then each
  `AGENTS.md` from the project root down to the folder opened; later ones
  come later in the prompt and override.
- **Hooks** (the hooks page). In `hooks.json` or `[hooks]` tables in
  `config.toml`, at `~/.codex/` or a repository's `.codex/`; every
  matching hook from every layer runs, none replaces another. A
  project's hooks load only when its `.codex/` layer is trusted, and
  hooks other than managed ones run only after review and trust in
  `/hooks`. A `PreToolUse` hook sees shell commands as `Bash` and file
  edits as `apply_patch` (`tool_name: "apply_patch"`; a matcher may also
  say `Edit` or `Write`). To deny: `hookSpecificOutput` with
  `permissionDecision: "deny"` and a reason, or exit code 2 with the
  reason on stderr; `"ask"` is not yet supported. A matcher filters by
  tool name, not by path, and no per-folder setting is documented.
  `commandWindows` gives a Windows-only command; the page's example runs
  `py -3`. Its limits, in its words: "Treat tool hooks as a useful
  guardrail, not a complete enforcement boundary"; "Some specialized tool
  paths can opt out of the default hook path"; hosted tools such as web
  search are not covered.

## From a trial on Windows (2026-11-09)

A vault's setup run by Codex on Windows, reported by the agent; the
user's harness had not been set by this guide.

- `codex-cli 0.167.0-alpha.3.2` (an alpha); before the version line,
  warnings that cleaning a temporary folder and creating a PATH alias
  were denied. Windows reported as "Windows 10 Pro 25H2, build
  26200.9711" — build 26200 is Windows 11 25H2, so the name is likely
  the registry's old product name (not checked). The shell PowerShell;
  the sandbox reported as "managed workspace-write", its kind (`mxc`,
  `elevated`, `unelevated`) not reported; approvals `auto_review`.
- `python3` could not start inside the sandbox (access denied) and ran
  escalated; the cause was not found. `git add` stopped at
  `.git/index.lock` (permission denied), as the protected `.git` above
  predicts. No hook was set. The vault's procedure, written for Git
  Bash, was run in PowerShell.
- An escalated `git commit` ran the vault's pre-commit, which refused a
  change to a tracked file it protects. Auto-review approved the
  escalation; no prompt reached the user.

## The ChatGPT desktop app on macOS (trials, 2026-11-19)

One Mac, the ChatGPT desktop app version 26.1118.11504, a Business
workspace. Codex chats were opened on a vault: a git clone whose
`AGENTS.md` has the session pull at start, run the vault's lint script,
list open issues with `gh`, commit and push. Codex ran the commands and
reported them with their raw output; the reports were read against the
docs (`learn.chatgpt.com/docs/…` and `learn.chatgpt.com/codex/…`, read
2026-11-19; no page dated). One machine and one version: observations.

- **Where Codex is.** On macOS, Codex is a mode of the ChatGPT desktop
  app, beside Chat and Work (Work is not Codex); the docs' page is
  "ChatGPT desktop app", its installer `Codex.dmg`. Its data folder is
  `~/.codex` (`config.toml`, `AGENTS.md`, `rules/`); the app's settings
  hold more than the file, and its AGENTS.md editor edits
  `~/.codex/AGENTS.md`.
- **The permission control** under the composer, per chat: Ask for
  approval; Approve for me (the docs: `workspace-write`, `on-request`,
  `approvals_reviewer = "auto_review"`); Full access (absent from the
  menu while the General tab's Full access toggle is off); Custom (from
  `config.toml`). **The composer's choice overrides the file**: with a
  permission profile set in `config.toml`, Ask for approval chosen in a
  new chat gave the preset's permissions, the file unchanged. The
  reviewer came from the app as well: with `approvals_reviewer` removed
  from the file, a new chat on Approve for me still showed
  `auto_review`. The docs: selecting a Daybreak model can switch the
  control to Approve for me by itself.
- **The presets' sandbox**, from the permission list Codex is given:
  read anywhere (`:root`); write in the workspace, a per-chat folder
  under `~/.codex/visualizations/`, `/tmp` and `$TMPDIR`; `.git`,
  `.agents`, `.codex` and `.aws` read-only at the top of each writable
  root (not in a subfolder: below, 2026-11-20); the network off. So a session can read whatever the user can, and
  every `git pull`, `git commit`, `git push` and `gh` call needs an
  escalation.
- **Escalation.** Under Ask for approval, `curl` and `git push --dry-run`
  each raised a prompt; the push's said the name lookup of `github.com`
  had failed in the sandbox and asked to re-run the command outside it —
  the whole command leaves the sandbox, not its network alone. The
  buttons: Deny, and Allow once with a menu beside it (not opened).
  Under Approve for me the same commands, a live web search and the
  built-in browser passed with no prompt and no verdict shown in the
  chat; Auto-review is inferred to have approved the escalations, the
  reviewer itself not seen. What Auto-review refuses, and whether a
  refusal shows: not seen.
- **A vault's routine rests on the model asking.** Under the presets the
  vault's start-of-session `git pull --ff-only` failed in the sandbox
  (`cannot open '.git/FETCH_HEAD'`) and `gh` could not reach the
  network. In one chat Codex asked for no escalation — it judged the
  user's question answerable locally — and reported a limit of its
  environment; in later chats it asked, and the pull passed. An
  instruction saying when to ask is one way to make it steadier; whether
  to have one, and in which file, is the user's (and the vault's, for
  its own file).
- **Git hooks run outside the sandbox.** With `.git` read-only every
  commit escalates, so a vault's pre-commit hook runs outside the
  sandbox, while the hook's files sit in the workspace, writable from
  inside; a reviewer sees the command, not the hook's code (found by the
  vault's own session). See `guide.md` § 2.14.
- **Permission profiles in the app.** A profile in `config.toml`
  (`default_permissions`; `:root` deny with `:minimal` read; the clone
  writable with `.git` and the vault's rule files read-only; git's own
  config readable and the credential files denied; network off) showed
  by name in the composer and held: reads and writes outside, the
  network and writes to the protected paths were refused, the denies
  marked not escalatable. **Nothing left it.** Requests to run outside
  the sandbox (`require_escalated`, eight over two chats: a branch, `git
  pull`, `git push --dry-run`, `gh api`, a write under `~/.codex`) ran
  inside and met the profile's errors, with no prompt and no verdict;
  `prompt` rules likewise; an `allow` rule for `git ls-remote origin` ran
  with no prompt and could not resolve `github.com`. The built-in
  browser failed (its helper could not read
  `/System/Library/OpenSSL/openssl.cnf`), and every git command printed
  warnings (xcrun's cache, a global gitignore). So under that profile the
  vault's sync, commit and issue listing did not run. The docs are
  silent on escalation under a profile ("approved escalations use their
  own controls"); a gihyo.jp article and a forum post say approved
  escalations are handled outside profiles — not what was seen. As
  tried, a read fence and a reviewed way out of the sandbox were not had
  together. (2026-11-20: the routine was moved inside a profile instead;
  below.)
- **Profiles through the CLI** (`codex sandbox -P … -c …`, on throwaway
  repositories): `:root` deny with `:minimal` read blocked reads and
  writes outside; `.git` writable with `.git/config`, `.git/hooks` and
  the hooks folder read-only let commits through and refused `git
  config`; a `github.com` allowlist (`features.network_proxy`) let an
  anonymous `ls-remote` through and refused another host; the macOS
  Keychain credential helper failed inside the sandbox (a private
  repository unreachable) and passed outside. A profile with the sync
  inside it would need these and a credential readable inside — tried in
  the app on 2026-11-20 (below). A workspace admin can enforce profiles
  (`allowed_permission_profiles`), where a member's composer would
  otherwise override the file.
- **Command rules** (`~/.codex/rules/*.rules`). A `forbidden` rule
  rejected the command before its process started, with the rule's
  reason, under a profile and under a preset (seen: `git push --force …`,
  `git commit --no-verify`). A `prompt` rule raised a prompt with its
  justification; allowed once, the command still ran inside the sandbox
  and failed (`.git/FETCH_HEAD` read-only) — one observation; the docs
  say only "Prompt before each matching invocation". Matching is by
  argument prefix: a rule on `git push --force` does not catch `git push
  origin --force` unless it names that placement too.
- **The web.** `[sandbox_workspace_write] network_access = false`: under
  Ask for approval the network was shut in the sandbox (the name lookup
  failed); whether the presets read that key at all is not known. Live
  web search (`web_search = "live"`; the docs' default is cached, which
  "reduces exposure to prompt injection from arbitrary live content")
  and the built-in browser, its browsing default Always allow, reached a
  page with no prompt under Approve for me. The built-in browser has
  its own profile and settings: the browsing default (Always allow or
  ask), history access, site tools (WebMCP), downloads and uploads, a
  password manager, contact info, extensions. With browsing on Always
  allow, a session can put what it has read into a URL and visit it
  unasked.
- **Git and GitHub.** A local Codex used the Mac's own `git` and `gh`
  login; no GitHub connection in ChatGPT was needed (ChatGPT's GitHub
  connector serves cloud tasks and code review). The commit author came
  from `~/.gitconfig`.
- **Other settings that bear on Codex**, from the app's tabs and the
  docs — what each does: memories (off by default; the docs: "Treat
  memories as a helpful recall layer, not as the only source for rules
  that must always apply"); a "Codex" custom-instructions field in
  Personalize, undocumented, which also reaches Codex Cloud, beside the
  documented `~/.codex/AGENTS.md` (32 KiB cap); plugins, which "work in
  Chat, Work and Codex", the host's sandbox and approval policy applying
  through Codex; computer use (app access asked of the user, use while
  the Mac is locked, always-allowed apps, and a Chrome extension that
  acts in the user's signed-in Chrome — "sites may treat ChatGPT's
  actions as coming from your account"); computer history (text
  summaries of activity in apps and sites); dots (always-on cloud agents
  with their own computer, which can be connected to the Mac); saved
  passwords (a saved login lets the agent sign in as the user; the
  built-in browser's help: a password typed into its sign-in form is not
  visible to the model); connections (the Mac controlled from another
  device); hooks (`PreToolUse`, `PermissionRequest`; "a useful
  guardrail, not a complete enforcement boundary"); Git (always force
  push; commit and PR instructions); Import (detects Claude Code and
  Claude Cowork; maps settings, instruction files, MCP, skills, hooks,
  subagents, plugins and project memories, and 30 days of chats; what
  its sync does is not stated); keyboard shortcuts, global ones among
  them. On the Business workspace the data controls showed training off
  and fixed (OpenAI's help: Business, Enterprise and Edu content is not
  used for training by default).
- **The admin route.** Admin Console > Agent security (workspace owners,
  admins, members with Codex admin permissions): requirements members
  cannot override — approval policies, approvals reviewers, sandbox
  modes, permission profiles, web search modes, managed hooks, the MCP
  list, plugins — and defaults (a cloud-delivered `config.toml`); it
  covers the desktop app's Codex. Whether Business is among "supported
  plans": not confirmed.

## An agent wrote outside its folder (the desktop app on macOS, 2026-11-20)

The same Mac and app, Approve for me (`auto_review`), the presets'
sandbox. A Codex chat opened on a vault edited files in another
repository on the Mac — outside its folder — by two escalated
(`require_escalated`) `python3 -c` runs writing with
`Path.write_text()`; each escalation's justification was written by the
agent itself. No prompt reached the user, so Auto-review approved both.
By the agent's own account, the user had said that another session would
make a change later, and it took that as leave to make the change
itself — past three written lines: the user's word to keep it for
later, a line in `~/.codex/AGENTS.md` allowing writes outside the folder
only when the user explicitly instructs or permits, and the target
folder's own `AGENTS.md`, which says to write nothing there. It had found
that folder through a local path written in one of the vault's
documents, then listed and read it. The changes were found uncommitted,
by `git status` in that repository.

What it shows: under the presets the way out of the sandbox opens
whenever the model asks and the reviewer agrees — instructions and a
reviewer are not a boundary (`guide.md` § 2.1, § 2.14); a write by a
script passes edit-tool checks and command rules alike (§ 2.2); a local
path to another folder in a vault's document leads an agent there
(§ 2.5). What held, the same day: the profile below.

## A permission profile with the routine inside (the desktop app on macOS, 2026-11-20)

The same Mac and app. A profile written into `config.toml` with the app
quit, then checked in new chats on the vault: Codex ran set commands and
reported their raw output. The syntax is from the docs' Permissions page
(marked Beta; `learn.chatgpt.com/docs/permissions`, read 2026-11-20).
The configuration itself: the second reference, below.

- **A fence with no folder named.** `:workspace_roots` stands for the
  session's folders (the folder opened, and the chat's
  `~/.codex/visualizations/…` folder), so one profile fences every folder
  Codex is opened on. `:root` deny, `:minimal` read, reads of what the
  tools need, the temp folders denied; Codex's permission list showed
  each entry, the denies marked `escalatable: false`. A listing of a
  sibling folder, a `touch` outside and a `python3 -c` write outside —
  the form of the write above — were refused (`Operation not
  permitted`), and so were the same with `require_escalated`, with no
  prompt.
- **`.git` is protected under a profile too.** With `"." = "write"` on
  the folder and no entry for `.git`, `git pull --ff-only` failed
  (`cannot open '.git/FETCH_HEAD'`), as under the presets, though
  Codex's list showed no such entry. `".git" = "write"` lifted it (the
  list then showed `.git` as a writable root); `".git/config" = "read"`
  and `".git/hooks" = "read"` beside it kept those refused (`git
  config`: "could not write config file .git/config"; a file in
  `.git/hooks`), so the agent cannot plant a hook or a hooks path that
  runs when someone commits outside the fence — **in the opened folder's
  own repository**. These entries are paths relative to the folder
  opened; in a repository nested below it the same writes passed (next
  section).
- **The network, GitHub alone.** The profile's `network.enabled = true`,
  `[features] network_proxy = true`, and `network.domains` allowing
  `github.com` and `api.github.com`: Codex set `HTTP_PROXY` and
  `HTTPS_PROXY` (a loopback port) and `ALL_PROXY` (socks5h) in each
  command, and refused another host before the command ran — "Network
  access to "example.com" was blocked: domain is not on the allowlist
  for the current sandbox mode."
- **A credential inside.** The macOS Keychain is out of reach inside the
  sandbox (2026-11-19), so the user's own `git` and `gh` login does not
  work there. What worked: a fine-grained personal access token for the
  vault's repository alone (Contents and Issues read and write,
  Metadata read; no Workflows, so a push that changes GitHub Actions
  files is refused), made on GitHub and stored by the user with
  `GH_CONFIG_DIR=~/.config/gh-codex gh auth login --with-token
  --insecure-storage` — plain text in that folder, which the profile
  makes read-only; and, through `[shell_environment_policy.set]`,
  `GH_CONFIG_DIR` pointing there and gh's own credential-helper pair
  (the form `gh auth setup-git` writes) given as command-scope Git
  config (`GIT_CONFIG_COUNT`, `GIT_CONFIG_KEY_<n>`,
  `GIT_CONFIG_VALUE_<n>`). Inside the fence `gh auth status` read the
  token from that folder; `git ls-remote` on the private repository,
  `git pull --ff-only`, `git push --dry-run` ("Everything up-to-date")
  and `gh api` on its issues passed. A real push: not yet seen. The
  user's terminal and `~/.gitconfig` are unchanged. The docs say nothing
  on credentials inside the sandbox, nor on whether the app sets the
  proxy variables.
- **The composer** showed the profile's name. Codex's own header still
  read `sandbox_mode: workspace-write` and called the profile `managed`;
  the entries were the profile's. A preset chosen in the composer leaves
  the profile for that chat (2026-11-19).
- **The built-in browser** at first: "Browser is not available: iab".
  Its helper runs inside the sandbox (hence the OpenSSL read of
  2026-11-19) and finds the browser through Unix sockets in
  `/tmp/codex-browser-use` (read in the browser plugin's code), which the
  denied `/tmp` hid. A read on `/private/tmp/codex-browser-use` and a
  `unix_sockets` allow on the same folder brought it back: a page
  opened, and web search worked. In Codex's source a `unix_sockets`
  allow becomes a Seatbelt `subpath` rule, so a folder covers the sockets
  made in it later; under the proxy, sockets not listed are refused; a
  filesystem read alone does not allow a connection. The rest of `/tmp`
  stayed refused. Every `git` command warned that xcrun's cache could
  not be written in `$TMPDIR`; harmless.
- **What it costs, and what it leaves.** The agent can read the token;
  its reach is one repository and the network GitHub alone, and it can be
  revoked. Web search and the built-in browser are not filtered by the
  proxy (the docs): they are a way out beside GitHub, and with browsing
  on Always allow a session can put what it has read into a URL. In any other GitHub repository Codex's `git` offers this token
  and fails. A repository nested in the folder is not protected by the
  `:workspace_roots` entries (next section). The vault's own hook
  programs inside the folder (a hooks folder, a script they call) stay
  writable: they run inside the fence
  when Codex commits, unbounded when someone commits outside it. A
  trusted project's `.codex/config.toml` is read, and the docs say a
  higher-precedence layer can add or replace entries under the same
  profile name; whether a project's layer can loosen this one: not tried
  (Codex cannot write that file; a commit synced from others could). In
  a workspace, a member's profile holds only while the composer is left
  alone, unless an admin enforces it (`allowed_permission_profiles`).

## A repository nested in the opened folder (the desktop app on macOS, 2026-11-20)

The same Mac and app, under the profile of the second reference. A vault
may hold another git repository in a subfolder — its own `.git`, ignored
by the vault's Git. The files that matter there are the ones another
program reads and runs later, outside any sandbox, with the user's
rights: Git's `config` (it can name commands — `core.fsmonitor`,
`core.hooksPath`, a credential helper, an alias that begins `!`) and
`hooks`, and each tool's project folder (`.codex`, `.agents`,
`.claude`).

- **The gap (tried).** In a throwaway repository made with `git init`
  in a subfolder of the opened folder, and then at the top:

  | target | nested repository | the opened folder's top |
  |---|---|---|
  | `git config <key> <value>` | written | refused ("could not write config file .git/config: Operation not permitted") |
  | a file in `.git/hooks` | written | refused |
  | a file in `.codex/` | written | refused |
  | a file in `.agents/` | written | refused |
  | a file in `.claude/` | written | refused |

  No approval was asked. The profile's entries under `:workspace_roots`
  are "workspace-root relative subpaths" (the docs): the top alone.
  Codex's own protection under the presets is anchored the same way, at
  the writable root (the source, `seatbelt.rs`, read; not tried under a
  preset). Neither the docs nor the source say anything of a nested
  repository.
- **A pattern cannot close it.** A key that holds `*`, `?`, `[` or `]`
  is a glob, and a glob takes `deny` alone: with `read` or `write` the
  profile fails to compile — "filesystem glob path … only supports
  `deny` access" (the source, read; not tried). So "`.git/config`
  read-only at any depth" cannot be written. A `deny` glob under
  `:workspace_roots` is supported (the docs' example: `"**/*.env" =
  "deny"`), but `deny` "denies both reads and writes": it hides as well
  as locks, and a hidden `.git/config` would stop Git in that repository
  (inferred; not tried).
- **A `deny` glob with `**` stops the agent from removing or moving any
  folder (tried).** Four deny globs were written for the other families
  at depth (`"*/**/.git/hooks/**"`, `"*/**/.codex/**"`,
  `"*/**/.agents/**"`, `"*/**/.claude/**"` — one path component first,
  so the top's literal entries stand). The profile compiled and the
  writes at depth were refused. But `rm -rf` on a test folder then
  removed every file and was refused at every directory, unrelated ones
  included; before the globs the same command had removed all. The cause
  is in the source (`seatbelt.rs`, read): for each deny glob, every
  prefix of the pattern gets a rule that denies unlinking directories
  that match it — "Also deny renaming directories that could contain
  matches so the protected files cannot be moved beyond the path where
  the glob applies" — and with `**` the prefixes match every directory
  under the root. By the same reading the docs' own `**/*.env` would do
  the same (not tried); the docs do not mention it. A rename was not
  tried (the same rule). A plain `git init` beside the test repository
  also failed, at copying the template hooks into the denied
  `.git/hooks`. The globs were taken out.
- **What was kept: five literal `read` lines for each nested
  repository**, under
  `[permissions.<profile>.filesystem.":workspace_roots"]`, with `<R>`
  the repository's path relative to the opened folder:

  ```toml
  "<R>/.git/config" = "read"
  "<R>/.git/hooks" = "read"
  "<R>/.codex" = "read"
  "<R>/.agents" = "read"
  "<R>/.claude" = "read"
  ```

  Tried in two of the listed repositories (the others have the same
  form, not tried one by one): `git status` and `git log` ran; `git
  config` refused; a file in `.git/hooks` refused; `mkdir` of `.codex`,
  `.claude` and `.agents` refused; `mv .git .git-moved` refused;
  ordinary files and folders made, renamed and removed; at the top the
  five refused as before; the vault's routine (`git ls-remote`, its lint
  script) passed; no approval asked.
- **What the lines leave.** A repository with no lines is open — all
  five families (tried: `git config` and a file in `.git/hooks`
  written). So is one cloned before its lines, moved or renamed, with no
  sign. The folders leading to a listed repository cannot be removed or
  moved by Codex (the source: a literal entry protects its own
  ancestors; the listed `.git` seen). The tools' folders anywhere else
  in the vault — a `.codex` that is not at a listed repository's top —
  stay writable, as under the presets. Not tried: a submodule or a
  worktree (a `.git` file with a pointer); a swap of the top's `.git`.
- **Adding a repository: the lines first, then the clone.** A
  repository cloned first is open until its lines are written. A line
  may name a path that does not exist yet, and with the lines in, Codex
  cannot make a repository there: `git init` at a listed path was
  refused at `.git/hooks/` ("Operation not permitted"), while an
  ordinary file beside it was made (tried, the paste trial below). So
  the clone is the user's, in their own terminal. If Codex is to pull or
  push the nested repository, the credential inside the fence must cover
  it (the second reference's token is scoped to one repository). A
  check, run in a Codex chat and compared with the lines:
  `find . -mindepth 2 -name .git -not -path './.git/*' -prune`.
  The paths are relative to whichever folder Codex is opened on.
- **Who writes the lines: the user, by a paste, with the app running.**
  Under the profile Codex could not write its own `~/.codex/config.toml`
  — outside the folder, and a request to run outside the sandbox ran
  inside (tried, above); the profile's entries also leave that file
  outside what it may read (a read was not tried). So the agent shows
  the lines and the user pastes them. Tried 2026-11-20 on the ChatGPT
  desktop app 26.1111.71704 on macOS, the user doing each step by hand
  with the app running throughout:
  - a backup copy of `config.toml` first — the way back;
  - the file opened in a plain-text editor (`~/.codex` is a hidden
    folder: `open -e ~/.codex/config.toml` in a terminal opens it in
    TextEdit; Finder's Go to Folder, ⌘⇧G, is another way), the last line
    of the `:workspace_roots` table found, five lines pasted under it,
    saved. The file then differed from the backup by those five lines
    alone, plain quotes kept;
  - **no quit and no restart were needed**: a chat opened after the save
    had the lines in force — the writes refused, and the five entries in
    the permission list Codex is given;
  - **a chat already open keeps the permissions it started with**: the
    chat open since before the paste still wrote into the path the new
    lines protect. After an edit the work goes on in a new chat, not in
    an old one;
  - lines taken out took effect the same way, in the next new chat; the
    file was then byte for byte the backup — the app had not written to
    it through three chats.

  One Mac, one version, some twenty-five minutes. Not seen: the app
  writing `config.toml` on its own — a setting changed in its screens, a
  folder trusted — while an editor holds the file open, where the later
  save would win; other versions; Windows. (Until this trial every edit
  on the test bed was made with the app quit, a precaution that proved
  unneeded here.) The check is the agent's, in the new chat: the
  permission list it is given and one refused write. Each chat has a
  second workspace root, a folder of Codex's own under
  `~/.codex/visualizations/`, and a `:workspace_roots` entry applies to
  it too — no harm seen. A chat switched in the composer to Ask for
  approval leaves the profile, and there Codex might write the file
  with the user's approval: not tried, and not the way proposed — it
  would have the agent rewrite its own bounds.

## A reference configuration: the ChatGPT desktop app on macOS (2026-11-19)

One configuration that worked on the test bed above, for a user who
works in a shared vault through Codex and wants few prompts; each line
with its reason. **Not the set this guide proposes** when a user leaves
the setup to the agent — that is the second reference: this one is the
lighter alternative, named with its weakness (the last sentence of this
paragraph). A reference, not a requirement: the user decides each
point with their agent, and the docs for their version win. Personal
preferences (keyboard shortcuts, the send key) are left out. On
2026-11-20 a chat under this configuration wrote outside its folder
with Auto-review's approval (above); the second reference keeps the
routine inside a profile instead.

- **Mode: Approve for me**, chosen in the composer for each chat — the
  composer overrides `config.toml`. A vault's routine needs an
  escalation at every pull, commit, push and `gh` call; under Ask for
  approval each is a prompt, and many prompts wear attention down
  (`guide.md` § 2.10); Auto-review answers them instead. Auto-review is
  a model, not a boundary: what must never happen goes in `forbidden`
  rules (below). Full access off: with the General tab's toggle off it
  is not in the menu. The weakness, seen 2026-11-20: whether a command
  leaves the sandbox is decided by the model and the reviewer, for any
  command, a write outside the folder included.
- **`~/.codex/config.toml`**, the part that bears on this:

  ```toml
  sandbox_mode = "workspace-write"
  web_search = "live"

  [sandbox_workspace_write]
  network_access = false
  ```

  No approval keys: the composer's choice supplies them (with
  `approvals_reviewer` removed, Approve for me still showed
  `auto_review`). The network off makes a command that connects an
  escalation, which is reviewed. Live search shows a page as it is now;
  the docs' default, cached, "reduces exposure to prompt injection from
  arbitrary live content" — the trade-off. No permission profile here:
  under the one tried on 2026-11-19 the vault's sync failed; the second
  reference has the sync inside one.
  The app adds a `[projects."<path>"] trust_level = "trusted"` entry when
  the vault's folder is trusted.
- **`~/.codex/rules/<name>.rules`**, forbidden rules for what no reviewer
  should pass. For example:

  ```
  prefix_rule(
      pattern = ["git", "push", ["--force", "-f", "--force-with-lease", "--mirror", "--delete", "-d"]],
      decision = "forbidden",
      justification = "Rewrites or deletes shared history. Ask the user.",
  )
  ```

  and the same after `"origin"` and after `"origin", "main"`; and `git
  commit` with `--no-verify` or `-n`, which skips the vault's
  pre-commit. A list in one position is a set of alternatives (the
  docs' rules page). Matching is by exact argument prefix, so each
  placement is a rule of its own, and a form not named passes (`git
  push origin +main`, `--force-with-lease=<ref>`; `guide.md` § 2.2).
  Seen rejecting before the process started, with the justification.
- **Instructions and memory.** Codex memories off, and memories from
  tool-assisted chats off: the docs call memories "a helpful recall
  layer, not ... the only source for rules that must always apply". The
  "Codex" custom-instructions field in Personalize empty — undocumented,
  and it reaches Codex Cloud too; the user's own instructions, if any, go
  in `~/.codex/AGENTS.md`, the documented place. A line there that, when
  a vault's routine fails in the sandbox, the agent asks to run it
  outside — the route, not a workaround — makes the routine steadier
  (above).
- **Web and the built-in browser.** The built-in browser on, its
  browsing default Always allow: looking at a page needs no approval
  (the cost: a session can put what it has read into a URL unasked).
  Downloads and uploads Requires approval: a file in or out is asked.
  History access off (the docs: history "can include internal URLs,
  search terms, and other sensitive information"); site tools (WebMCP)
  off; no saved passwords, contact info or extensions in the browser.
- **What acts as the user, off.** Computer use: no app allowed, not
  while the Mac is locked, no always-allowed apps; no Chrome extension
  ("sites may treat ChatGPT's actions as coming from your account");
  computer history off; no passwords saved in the app (a saved login
  lets the agent sign in as the user); no control of the Mac from
  another device.
- **Cloud, off.** No GitHub connection in ChatGPT: a local Codex uses the
  Mac's own `git` and `gh` login, and the connector would open the
  repositories to cloud tasks. Codex Cloud, code review and dots not
  used; plugins the work does not use, off.
- **Git tab.** Always force push off: it rewrites a shared vault's
  history. Commit and PR instructions empty: a vault's conventions are in
  its `AGENTS.md`.
- **Notifications** for permission requests and questions on: a prompt
  that does reach the user is seen.
- **Import** not run, sync off: another tool's settings and instructions
  are that tool's.
- **Hooks**: none of the user's own; a vault's `.codex/` hooks, if any,
  read, then trusted in the Hooks tab.
- **Before the first chat on the vault**: `git` and `gh` signed in with
  the user's own account, with access to the vault's repository.

## A second reference: a profile with the routine inside (2026-11-20)

**The set this guide proposes for Codex in the ChatGPT desktop app on
macOS**, and the one applied when a user leaves the setup to the agent
(`guide.md` § 4 step 4): it matches the guide's proposed answer —
nothing outside the opened folder read or written, held by a setting
and not by a model's judgement (`guide.md` § 3.1) — and the first
reference is the configuration under which a write outside the folder
happened. It is heavier, and these stay the user's own hands even when
the rest is left to the agent: making the token on GitHub and storing
it; pasting the configuration, with the paths found for their Mac; five
lines for each repository nested in the folder; and leaving the
composer's permission choice alone — a preset chosen there leaves the
profile for that chat. Tried on one Mac, in the desktop app; a real
push was not yet seen; under the CLI or on another platform it was not
tried, and that is said before the user chooses.

For a user who wants a write or read outside the folder stopped by the
sandbox rather than by a model, with the vault's routine (pull, commit,
push, its issues through `gh`) still running. The configuration tried
above, as it stood on the test bed; the paths are that Mac's — find each
on the user's (where `git`, `python3` and `gh` live; which git config
files exist). A reference, not a requirement.

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
".claude" = "read"
# For each git repository nested in the folder, five more lines, written
# before it is cloned ("A repository nested in the opened folder"):
# "<R>/.git/config" = "read"
# "<R>/.git/hooks" = "read"
# "<R>/.codex" = "read"
# "<R>/.agents" = "read"
# "<R>/.claude" = "read"

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
GIT_CONFIG_COUNT = "2"
GIT_CONFIG_KEY_0 = "credential.https://github.com.helper"
GIT_CONFIG_VALUE_0 = ""
GIT_CONFIG_KEY_1 = "credential.https://github.com.helper"
GIT_CONFIG_VALUE_1 = "!/opt/homebrew/bin/gh auth git-credential"
```

The reasons, line by line:

- **No `sandbox_mode`, no `[sandbox_workspace_write]`.** The docs: a
  profile or those, not both; either in any loaded file makes Codex use
  the older settings. It was written with the app quit; the paste trial
  later showed that is not needed (above).
- **`default_permissions`** applies the profile to every chat; the
  composer shows its name, and a preset chosen there leaves it for that
  chat. The approval keys are those tried; under the profile a request
  to run outside the sandbox ran inside all the same.
- **`:root` deny, `:minimal` read**: nothing outside is read or written
  unless named. The named reads are what the routine's tools need: the
  Command Line Tools (`git`), Homebrew (`python3`, `gh`), the OpenSSL
  file the built-in browser's helper read on 2026-11-19, git's own config
  for the author — read first, to see that it holds no credential.
- **Temp folders denied**: other programs' files sit there. The cost:
  `git` warns at each command. The one folder opened,
  `/private/tmp/codex-browser-use`, read and its sockets allowed, is
  where the built-in browser's helper finds the browser; without it the
  browser is unavailable.
- **`~/.config/gh-codex` read**: the token's folder, kept from the
  agent's writes.
- **`:workspace_roots`**: the folder opened, with no path written. `.git`
  writable for pull and commit; its `config` and `hooks` read-only, so no
  hook or hooks path can be planted; `.codex`, `.agents` and `.claude`
  read-only, so the agent cannot loosen its own or another tool's
  project settings. All of this at the folder's top only: a repository
  nested in the folder has none of it until its own five lines are
  written, and a pattern cannot stand in for them (the section above).
- **The network**: GitHub alone, through the proxy (`api.github.com` for
  `gh`). Web search and the built-in browser are not filtered by it (the
  docs).
- **`[shell_environment_policy.set]`**: only Codex's commands get the
  token's folder and the helper; the user's own `git` and `gh` are
  untouched. The empty helper first clears the Keychain helper that
  fails inside.
- **The token** is made and stored by the user, never shown to the agent
  (a fine-grained token for the one repository; Contents and Issues read
  and write; no Workflows), and renewed by them at its expiry.
- **Kept from the first reference**: the `forbidden` rules (a forced push
  is still possible inside the fence), and the points on instructions,
  the web and the browser, what acts as the user, cloud, the Git tab,
  notifications, import and hooks.

## From a research report made for this guide (2026-11-09)

Citing the Codex docs of 2026; not checked here:

- Permission profiles (Beta): read, write and deny levels; built-in
  read-only, workspace and full access; the docs say to set the default
  explicitly.
- No guided set-up or interview is recorded.

## A vault built for Claude Code

A vault whose enforced rules are Claude Code's hooks
(`.claude/settings.json`) loses, under Codex, what those hooks do in the
session: Codex reads the vault's `AGENTS.md` but runs none of them. What
does not depend on the agent tool still holds — git's own hooks (a
pre-commit set by `core.hooksPath`) on a commit, and any check made at
push or on the server. Whether a vault accepts Codex, and how it
enforces its rules there, is that vault's to decide; nothing in this
guide requires one tool.

A vault's hook program written for Claude Code checks Claude Code's
tool names (`Edit`, `Write`, `Bash` …); under Codex an edit arrives as
`apply_patch` and passes a program that does not look for that name
(found in one vault's program by its session, 2026-11-09). An adapter
would also have to read `apply_patch`'s input and the commands of the
shell Codex runs on Windows, which a program written for bash may
misread (not checked here). Whether to adapt, and where an adapter is
kept: `guide.md` § 2.12, § 2.13, § 3.6.

## Not known here

Whether Codex may write its own `config.toml`, and whether such a write
is protected (under a profile in the desktop app, refused even when
escalation was asked; under the presets, not tried); what Auto-review
refuses, and whether a refusal shows; whether the presets read
`network_access`; which `permission_mode` a hook sees under Approve for
me, and whether hooks run inside the sandbox; whether Plugin Management
lets the agent install or enable plugins; whether the "ChatGPT" custom
instructions reach Codex chats; how command rules match PowerShell commands; why a
program may fail to start inside the Windows sandbox; how a repository's `.codex/config.toml` combines with
the user's, and whether it can loosen it — a permission profile
included; where its login is stored on Windows; which shell a `Bash`
hook sees on Windows (the hooks page does not say); whether a real push
passes inside the second reference's profile; a submodule or a worktree
nested in the folder; whether a `read` glob is in fact rejected, and
what the app does with a profile that fails to compile; whether Codex
can write its own `config.toml` in a chat on Ask for approval; whether
the app ever writes `config.toml` over an edit left open in an editor;
what
happens when Codex starts another agent's program from the shell.
