# Codex

What has been found here about Codex, dated and sourced: the docs as
read on 2026-11-09 and 2026-11-20, a vault's setup on Windows
(2026-11-09), trials in the ChatGPT desktop app on macOS (2026-11-19
to 2026-11-23). What was tried and what was only read — in the docs or
in Codex's source — is marked. This note is the evidence: the route
for the desktop app on macOS is `setup/codex-macos.md`, which points
here by section. **Since 2026-11-23 that route is the app as it
starts, on Ask for approval, with no file of bounds set** ("The app
as it starts: Ask for approval"; a run of a vault's sheets on it:
"Two sheets run on that route"); since 2026-11-25 it puts one line
into the user's own instruction file, a form not yet run. **The sections on a permission profile —
"A permission profile with the routine inside", "A repository nested
in the opened folder", "A tracked file under a read-only folder", "A
setup followed from its first step" — are the record of the route
before it**, kept for what they found; why it was left is in "A
profile that bounds reads gave way". Codex's own protection of Git's
files and of its project settings holds at the top of the opened
folder only. Which settings and guardrails a user has is theirs to
decide with their agent (`guide.md`). Where a note and the docs differ,
the docs win; Codex changes with each version.

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
  <file> -- <command>` shows the decision. A list in one position of a
  pattern is a set of alternatives.
- **Settings.** The user's `config.toml` is in `CODEX_HOME` (`~/.codex`;
  on Windows, for example, `C:\Users\<name>\.codex`); a repository may
  carry `.codex/config.toml`; `trust_level = "untrusted"` for a project
  disables its project-local configuration. A permission profile and
  the older sandbox settings (`sandbox_mode`, `[sandbox_workspace_write]`)
  are not used together: either of the older keys in any loaded file
  makes Codex use the older settings (the Permissions page, read
  2026-11-20).
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
  `~/.codex/AGENTS.md`. The Configuration screen shows values of
  `config.toml` — the approval policy, the sandbox, network access, web
  search — and opens the file. When a folder is trusted the app adds a
  `[projects."<path>"] trust_level = "trusted"` entry to the file by
  itself.
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
  them; bundled Node.js and Python ("Codex dependencies"), Plugin
  Management and worktrees (none of the three judged here); the app's
  own MCP servers `node_repl` and `codex_apps` (the built-in browser
  and computer use run on the first, by GitHub issues, not the docs).
  On the Business workspace the data controls showed training off
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
(§ 2.5). What held, the same day: the profile of the next section.

## A permission profile with the routine inside (the desktop app on macOS, 2026-11-20)

The same Mac and app. A profile written into `config.toml` with the app
quit, then checked in new chats on the vault: Codex ran set commands and
reported their raw output. The syntax is from the docs' Permissions page
(marked Beta; `learn.chatgpt.com/docs/permissions`, read 2026-11-20).
The configuration itself, each line with its reason, was step 3 of
`setup/codex-macos.md` as it stood before 2026-11-23 (this
repository's history).

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
  token here was made by the repository's owner; who else can make one,
  and what is left for a user who cannot: `github.md`. The
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
  (Codex cannot write that file; a commit synced from others could).
  Read in the source since (2026-11-21, `codex-rs/config/src/merge.rs`,
  `loader/mod.rs` in part): layers are merged table by table, the
  higher layer's value winning on the same key, a trusted project's
  file above the user's; the list of keys a project file may not set
  (`PROJECT_LOCAL_CONFIG_DENYLIST` — the provider and base-URL keys,
  `notify`, `profile`, `profiles`, `otel` …) does not hold
  `permissions` or `default_permissions`; an untrusted or unknown
  project's file is loaded disabled. By that reading a trusted
  project's file could add to a user's profile or loosen it; the docs
  say neither, and the function that applies the list was not read. In
  a workspace, a member's profile holds only while the composer is left
  alone, unless an admin enforces it (`allowed_permission_profiles`).

## A repository nested in the opened folder (the desktop app on macOS, 2026-11-20)

The same Mac and app, under the profile of the section above. A vault
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
  script) passed; no approval asked. (Since 2026-11-21 the set in
  `setup/codex-macos.md` has four of these; the `.claude` line is
  optional — *A tracked file under a read-only folder*, below.)
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
  it (the token tried was scoped to one repository). A
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

## A tracked file under a read-only folder (2026-11-21)

Raised by a vault's agent writing its procedure: the vault tracks files
under `.claude/` (a settings file, skills), the profile kept `.claude`
read-only, and a session's routine pulls at its start. What was done
about it under the profile — a pull in three steps — was in step 3 of
`setup/codex-macos.md` before 2026-11-23 (this repository's history).
On the present route a pull the user allows runs outside the sandbox,
where these folders are not read-only (inferred; not tried).

- **The pull does not fail cleanly (tried, not under Codex).** Git
  2.54.0 (Apple), a throwaway origin and clone; tracked files in two
  folders made read-only by file permissions (`chmod -R a-w`) and in an
  ordinary folder; an incoming commit that changes one file in each and
  adds one in a read-only folder and one in the ordinary folder.
  `git pull --ff-only` fetched, then failed, exit 1: "error: unable to
  unlink old '.claude/settings.json': Permission denied", "error:
  unable to create file .claude/skills/new.md: Permission denied".
  After it: HEAD not moved, the index unchanged, the branch one commit
  behind; the files in the read-only folders the old ones, with no
  change shown; the changed file in the ordinary folder already
  rewritten and shown as modified, the added one there as untracked. A
  second pull refused — "Your local changes to the following files
  would be overwritten by merge", "The following untracked working tree
  files would be overwritten by merge" — and so did one after the
  folders were made writable again. A `git add -A` there would take the
  two leftovers as local work on the old HEAD: a commit of them
  diverges from the remote, and the change under the read-only folder
  enters no commit, so nothing is silently reverted. Under Codex's
  profile the refusal would come from the sandbox ("Operation not
  permitted"); the same course is expected, not tried.
- **The way back (tried, the same clone).** With the folders writable
  — the user's own terminal — and no other uncommitted work: `git stash
  --include-untracked`, `git merge --ff-only @{u}`, `git stash drop`
  left the clone at the remote's commit, clean.
- **Looking before the merge (tried, the same way).** In a fresh clone
  one commit behind, the same folders read-only: `git fetch`, then
  `git diff --name-only HEAD @{u} -- <the read-only folders>` listed
  the three files of the incoming commit that lie there, and changed
  nothing in the clone. `git merge --ff-only @{u}` needs no network, so
  a user can run it in a terminal that holds no credential for the
  remote.
- **No line tells Git's write from the agent's.** A profile's entry
  binds every process in the sandbox; the access values are `read`,
  `write` and `deny` alone (the source, read).
- **What Codex loads from these folders by itself (the docs, read
  2026-11-21; not tried).** `.agents/skills/`: scanned in every folder
  from the working directory up to the repository's root; "Codex
  detects skill changes automatically"; a skill is "a directory with a
  `SKILL.md` file plus optional scripts and references", and Codex
  starts each chat with every skill's name and description, loading the
  body when it chooses the skill. No confirmation and no trust
  condition for a project's skills was found. `.codex/`: `config.toml`
  and `hooks.json` from a trusted project, hooks only once reviewed
  (above). `.claude/`: nothing — no page read names it; `CLAUDE.md` is
  read only if listed in `project_doc_fallback_filenames`, empty by
  default; the import from Claude Code is started by the user (the
  desktop app has a switch for automatic updates, off on the test bed).
- **The `.claude` line was this guide's own.** Codex's presets protect
  `.git`, `.agents`, `.codex` and `.aws` at the top, not `.claude`. On
  the test bed the profile kept `.claude` read-only too, on the ground
  that a file there is read by another tool; no problem had arisen from
  it. Since 2026-11-21 it is optional in `setup/codex-macos.md`: it
  guards a machine where Claude Code opens the same folder, and it
  costs the pull above in any vault that tracks files under `.claude/`
  — which a vault built for Claude Code usually does. Without it, a
  file Codex writes under `.claude/` and does not commit, or one Git
  ignores (`.claude/settings.local.json`), is seen by no check that
  compares commits; on a machine with Codex alone nothing reads it.

## A setup followed from its first step (the desktop app on macOS, 2026-11-22)

One user, one Mac, the ChatGPT desktop app 26.1117.41037 (its bundled
Codex CLI reported itself as 0.171.0-alpha.13), a Business workspace.
The user's `~/.codex` was set aside by a rename first, and the setup
was led by Codex itself from a vault's procedure made of this guide's
form of 2026-11-21 (one text to paste, a setup folder, the check in
further chats). Two runs, neither as far as the vault's clone. What
was seen, by the user's reports and screens:

- **With `~/.codex` set aside the app asked for a sign-in again** and
  listed no Codex chats; the ordinary ChatGPT chats were still there.
  A `config.toml` was there again soon after the first start, written
  by the app, with no profile in it.
- **The fresh state's settings.** General: Full access **on**. The
  Configuration screen opens on a scope selector set to Work; under
  the scope Codex: approval policy on request, sandbox read-only, web
  search cached; no network switch shown; the button that opens
  `config.toml` is under that scope.
- **A chat started with no folder chosen** is given a working folder
  by the app, under `~/Documents/Codex/<date>/` (the agent's own
  account of its working folder; the place is a setting under
  General).
- **An edit of `config.toml` in two stages failed between them.** With
  `default_permissions` and `approval_policy` pasted at the top and the
  `[permissions.…]` tables not yet in the file, the app showed `failed
  to load workspace requirements: invalid input parameter`. The lines
  were taken out and the file was again identical to its backup. The
  cause was not established: the file on disk at that moment was not
  captured, and that string is in neither the docs nor the source as
  read the same day — the nearest is the load of the workspace's
  cloud-managed policies, on whose failure config loading stops.
- **A profile put into a file that had none showed only after the app
  was restarted once.** (On 2026-11-20, lines added to a profile
  already in force held from the next new chat with no restart: "A
  repository nested in the opened folder", "Who writes the lines".)
- **Inside the profile, in chats not on the vault**: a file made and
  removed in the working folder; a backup outside it refused
  (`Operation not permitted`); `curl` to `example.com` refused (first
  `CONNECT tunnel failed, response 403`, then "domain is not on the
  allowlist"); `git ls-remote` on a private repository by its address
  passed with a member's fine-grained token, approved by the
  organization's owner — Git printed a refusal to write a cache, and
  the refs came all the same. In one chat an append to a file in
  another folder named as the working folder stopped with exit code
  134; from a chat opened on that folder the append passed.
- **The leading was lost.** The first chat led the user. The work then
  moved — to a new chat for the profile, to another for the check, to
  another on the setup folder — and each of those was handed a single
  request, took its part to be that request, and led nothing; the user
  carried texts from chat to chat, and a chat that took over through a
  record file did not know the repository's address and asked the user
  for it. Asked afterwards, the agent's own account was that it had
  said where the next check was to run and never who judged the result
  and went on. A user who does not question the agent stops there.
  Hence `guide.md` § 4, "The agent leads".
- **The model.** With the app's default model the agent left a written
  step (the file in one block) and changed a tried value unsaid; with
  a higher model the steps were kept, and the plan's five-hour
  allowance was used up in about half an hour. One run each: no more
  is known.

A third run, 2026-11-23, the same app, in the form of two sheets (the
model GPT-6.2 Sol, reasoning set to its highest by the user; the app's
default is medium):

- The first sheet — the app's settings and the harness — was led from
  a chat with no folder chosen to its end, the GitHub login included,
  from a logged-out state.
- The `config.toml` it left parsed, and held the profile and four
  lines for each of eleven nested repositories as the sheet gave them;
  the app's own keys were kept; the agent kept a backup beside it.
- The agent departed from the set of `setup/codex-macos.md` in two
  places, and said neither to the user before the file was saved; its
  record of the setup had a line for each. What each brought was then
  tried in a new chat under those settings, and the agent was asked
  why:
  - **No lines for the built-in browser** (the OpenSSL read, the
    socket folder's read and its `unix_sockets` allow). Tried: the
    built-in browser did not start — `node_repl kernel exited
    unexpectedly`, its diagnostics showing `OpenSSL configuration
    error … Operation not permitted … fopen(/System/Library/OpenSSL//openssl.cnf)`;
    a web search worked. The agent's account: the vault's routine, as
    it was given, had no browser in it, so it took the lines for an
    added permission and left them out; it did not try the browser.
    They are not the routine's: they put back a function of the app
    that the profile's own denies take away, in every chat.
  - **`GIT_CONFIG_GLOBAL` in place of the `GIT_CONFIG_COUNT` pairs**:
    `[shell_environment_policy.set]` named a file in the token's
    folder, holding `[include] path = ~/.gitconfig` and the two helper
    lines for `https://github.com`. Tried, inside the profile: Git
    read the name, the mail address and `core.excludesfile` from
    `~/.gitconfig` and the helper from that file; with a member's
    fine-grained token stored, `gh auth status` read it and `git
    ls-remote` on the private repository passed. Not tried: a commit,
    a pull, a push. The agent's account: the app already passes
    `GIT_CONFIG_COUNT` and pairs of its own to the commands it runs
    (ten, for ChatGPT's Sites), and the set's `GIT_CONFIG_COUNT = "2"`
    would have replaced them. Seen in the same chat: Git listed
    settings of scope `command` that the app had handed it — an empty
    helper, a helper `codex git-credential` and `usehttppath` for
    `https://chatgpt.com/backend-api/codex/sites/git/` and a staging
    host. Not tried: which side is replaced where both set those
    variables. Tried outside the profile (Git 2.54): where the named
    file is missing Git says nothing and no longer reads
    `~/.gitconfig` either — a commit then stops with "Author identity
    unknown".
- The lines for the vault's eleven nested repositories were written at
  the first setup, as the entry then had it, with none of them on the
  machine. A user's clone of that vault starts with no repository
  inside it and each user adds the ones they need: the lines of one
  are now written when it is added (`vault-procedures.md` § 3).
- A fresh `config.toml` written by the app enables, among its plugins,
  computer use and the browser, and gives the bundled CLI's place as
  `/Applications/ChatGPT.app/Contents/Resources/codex-cli/CodexCLI.app/Contents/MacOS/codex`.
- At the first sheet's end the agent told the user to go on by the
  second sheet, named by its title alone, and wrote a text of its own
  to hand over; the user, not knowing where that sheet was, stood
  still. And with the refusals left to the second sheet, a fault found
  there could not be mended by the chat that found it.

From the docs and the source, read on 2026-11-22 through a research
agent and not tried:

- **Configuration from the workspace.** Requirements bind the user
  (lowest to highest: `/etc/codex/requirements.toml`, the cloud bundle
  set under Agent security in the Admin Console, the legacy
  `managed_config.toml`, macOS managed preferences); they can carry
  `allowed_approval_policies`, `allowed_sandbox_modes`,
  `allowed_permission_profiles`, `default_permissions`, a profile's
  body, forbidden or prompting rules, hooks, features and the network
  proxy. "When present, the table is the complete list of allowed
  profiles." Cloud-managed defaults sit below the user's own
  `config.toml`. `shell_environment_policy` is not a requirement field
  — a default only — and its `set` values are taken literally, with no
  `~`. The pricing page marks cloud-managed policies for Business and
  Enterprise; "workspace owners, workspace admins, and members with
  Codex admin permissions can manage policies." Whether a profile so
  required leaves the composer's presets unavailable: the docs say
  only that a mode "can be disabled or omitted when it isn't
  available".
- **A command under the sandbox, with no chat**: `codex sandbox macos
  [--permissions-profile <name>] [--log-denials] COMMAND…` — "run a
  command under the same policies Codex uses internally"; with
  `default_permissions` set it uses that profile, and the network
  proxy is started for it (the source). The docs' troubleshooting page
  gives the binary's place as
  `/Applications/Codex.app/Contents/Resources/codex`.
- No command sets a key of `config.toml` in general (`codex features`
  and `codex mcp` write their own).

## A profile that bounds reads gave way (the desktop app on macOS, 2026-11-23)

The same Mac, the app 26.1117.41037 (its bundled Codex CLI 0.171.0).
A fourth run of the setup from its first step, in two sheets, under
the profile of "A permission profile with the routine inside": the
harness's sheet to the end of its check, then a vault's installation —
the token, the clone, the check on the vault — each to its end. Read
afterwards, the configuration held the set whole. Then, in ordinary
use:

- **A chat opened on a repository lost the opened folder at its second
  message (seen).** On a throwaway repository with no instruction
  file: at the first message the working folder and the writable roots
  were the repository; at the second, the working folder and the only
  root listed were the chat's own folder under
  `~/.codex/visualizations/`, and a file "made in the working folder"
  landed there. Nothing was shown: the agent went on in another
  folder. The profile names no folder — it reaches the opened one
  through `:workspace_roots` — so its bounds moved with the roots.
- **With an `AGENTS.md` in the folder the second message stopped**:
  `failed to load AGENTS.md instructions for environment `local`:
  Operation not permitted (os error 1)` — twice, in two new chats on
  the vault. Read in Codex's source (`codex-rs/core/src/agents_md.rs`,
  main branch): where a profile does not give read access to the whole
  disk, the instruction file is read through the sandbox and any error
  but "not found" ends the turn; under the app's presets the same
  error is only logged. So the stop is one that only a profile with
  restricted reads can have.
- **A chat with no folder chosen kept its roots** at its second
  message. Such a chat is given the whole of `~/Documents/Codex` as a
  workspace root beside its own folder, so that under the set's
  `"." = "write"` chats of this kind could write each other's folders;
  the setup agent added a `deny` on that folder by itself, and the
  chat's own folder still took a write.
- **A large pasted text was kept by the app as an attachment under
  `~/.codex/attachments/`**, which the profile closes to the agent:
  the text of the second sheet, pasted, became one object in the
  composer and the agent answered that it could not open the attached
  file; expanded into text by hand, it went through.
- **Not explained**: the chat that carried the vault's installation,
  begun on the folder while it was still empty, kept its folder to its
  end.

Reported in the tool's issue tracker (`openai/codex`; open, no
maintainer's comment seen; read through a research summary on
2026-11-23, not checked here): the loss of a project's root at a
follow-up turn with this error on the desktop app (#59438); the
instruction file's read failing closed under a profile since 0.149
(#40937); under `:root = "deny"`, every command refused after the
first in a chat with no project (#45953), a helper failing where the
profile cannot read Codex's own binary (#41493), a wrong root when a
chat starts in a subfolder (#54110), every sandboxed command aborting
where `:minimal` is not readable (#63214). The docs, re-read the same
day: permission profiles are Beta ("under active development and may
change"); the network proxy the domain list rests on
(`features.network_proxy`) is experimental and off by default.

What it shows: the profile was built on the part of Codex its vendor
marks unsettled, and an update of the app took a chat's folder from
under it. No way round was tried.

## What users of the app are found doing (read, 2026-11-23)

From a research summary made for this guide of public accounts —
vendor pages, guides, forum threads, published configuration files,
the tool's issue tracker; read on 2026-11-23, nothing tried, not
checked here; some sources could not be reached, and first-person
accounts of the desktop app over weeks were few.

- **Ask for approval**: what the vendor's page gives as the start
  ("For most work, start with Ask for approval",
  `learn.chatgpt.com/codex/permission-modes`) and what the guides for
  newcomers found give; no account was found of someone who stays on
  it over time.
- **Approve for me**: by the vendor's own account most of its internal
  desktop use, with the reviewer described as "not a security
  guarantee"; a handful of users' accounts.
- **Full access**: recommended by no one for daily use, kept for a
  disposable container or virtual machine.
- **A configuration of one's own**: a minority. What is published is
  `sandbox_mode = "workspace-write"` with `approval_policy =
  "on-request"`, the sandbox's network sometimes switched on so that
  `gh` and a push work. Of a profile that bounds reads: one person's
  test, and otherwise fault reports.
- **Git in the app**: the accounts found of a routine that works are
  of Git run by the user's own hand in a terminal; none of the app's
  own commit and push controls, none of a team, none of the app on a
  notes or knowledge-base repository with a stated permission choice.
- **Teams**: the vendor's admin guide (requirements a workspace's
  admin can fix: the permission choices allowed, who may answer
  approvals, profiles); no team's own account of running it.

The route of `setup/codex-macos.md` was chosen among these, not
derived: of the two choices found in use, Ask for approval is the one
under which leaving the folder is not a model's decision.

## The app as it starts: Ask for approval (the desktop app on macOS, 2026-11-23)

The same Mac, the app 26.1117.41037, a Business workspace. The user's
`~/.codex` and the folder of the earlier trials' token were set aside
by a rename with the app quit; the app was started again. Nothing was
set afterwards. The user ran each act and reported the screen and the
agent's raw output.

- **The choice a new install starts on.** A new Codex chat showed Ask
  for approval under the composer (the Japanese interface: 「承認を
  依頼」 there, 「承認を求める」 in the menu); the menu listed Ask for
  approval, Approve for me and Full access, and no "Custom". A new
  project made afterwards started on Ask for approval too.
- **Opening a folder.** The user made a new project and chose the
  folder as its "source folder" (「ソースフォルダ」).
- **A throwaway repository, first message.** `pwd`: the repository. A
  new file in it: made, no prompt. `git add` and `git commit`: inside
  the sandbox `fatal: Unable to create '…/.git/index.lock': Operation
  not permitted`; then one prompt, in the agent's words, asking to
  allow writing to `.git` and to run the two again; allowed once, the
  commit was made.
- **Second message, a write outside.** `pwd`: still the repository. A
  new file in the folder above it: a prompt asking whether to allow a
  write outside the workspace, naming the path, with the buttons
  refuse (「拒否」) and allow once; refused, the agent reported
  `exec_command failed: CreateProcess { message: "Rejected(\"rejected
  by user\")" }`, and the user, looking in their own terminal, found
  no such file. The request came before any attempt inside the
  sandbox.
- **A clone of a vault, the start of a session.** Asked to do the
  vault's start-of-session steps without committing or pushing, the
  agent raised two prompts: for `git pull --ff-only` (`.git/FETCH_HEAD`
  not writable inside) and for `gh api` on the repository's open
  issues (no connection inside). Each was allowed once and passed. The
  vault's script that builds its index ran with no prompt.
- **The prompt.** A sentence written by the agent ("As the vault's
  start procedure, allow writing to .git/FETCH_HEAD and run git pull
  --ff-only?"), the command under it, and two buttons: refuse (Esc)
  and allow once (「一度だけ許可」, Return). An arrow beside the second
  opens two choices, allow once and "allow similar commands"
  (「類似コマンドを許可」), the latter explained as allowing, in this
  conversation, commands that begin with a prefix the app names — `git
  pull --ff-only` for the pull, `gh api` for the reading of issues.
  Not pressed.
- **The login.** `gh auth status`, run by the agent inside the
  sandbox with no prompt: "Failed to log in to github.com account …
  (default)", "The token in default is invalid." Asked to be run
  outside it, with one prompt: logged in to the user's account
  "(keyring)", an OAuth token (`gho_…`) with the scopes `gist`,
  `read:org`, `repo`, `workflow` — the Mac's own login from `gh auth
  login`, not a token made for the agent. Git's credential helper, by
  `git config --show-origin`: `osxkeychain`, from the Command Line
  Tools' own configuration.
- **A push.** `git push --dry-run`: one prompt, saying that inside the
  sandbox the name `github.com` could not be resolved; inside, `Could
  not resolve host: github.com`; outside, `Everything up-to-date`.
- **Later messages.** The chat on the vault, whose folder holds an
  `AGENTS.md`, answered its second, third and fourth messages — the
  case that stopped under the profile.

So: a session with a start, a commit and a push is four prompts; a
write outside the folder is one more, and refusing it leaves nothing
written. Not tried: a real push; "allow similar commands"; a nested
repository under this form; the General switch for Full access off in
this state. The open reports for this version of prompts that do not
appear (#32563, #63187) did not show here.

## Two sheets run on that route (the desktop app on macOS, 2026-11-23)

The same Mac and app, later the same day. A vault's two sheets
(`vault-procedures.md` § 1, as it then stood: a first sheet for the
harness, with its check, then the vault's installation), rewritten
for the route above, were run by a user from their first act. The
user stopped the run at the end of the first sheet; the second was not
run. What the user said and what the setup chat reported at its end
are told apart; the agent's raw output was not seen.

- **Before the first act.** The app quit and `~/.codex` set aside by
  a rename, so that no project of the earlier trial was in the app;
  started again, **the app asked for the sign-in** (the user).
- **The first sheet's chat**, opened with no folder chosen. The tools
  and the GitHub login — `git`, `gh`, Python — were gone through in
  the user's terminal one at a time, without trouble (the chat's
  report).
- **The app's own settings were not gone through** (the user): no
  switch was named, the Full access switch among them. Why the chat
  left them out was not established. They stood in the route's file
  as recommendations, "none needed".
- **The check, in a chat made for it.** The first chat had no folder,
  so it had the user make a throwaway project on an empty folder:
  - told to stop at the project's creation screen and report, the
    user went back to the chat, **and the screen closed** (the chat's
    report);
  - in the check's chat the user asked what to do next, and no longer
    knew which chat led (the chat's report); the user had not been
    told, before moving, what the check was for or that a prompt was
    to be refused;
  - **the first try of a write outside the folder left an empty file
    there.** The command was stopped inside the sandbox; a request to
    leave it followed, and afterwards the file existed. What was
    chosen at the prompt was not recorded, and the user does not
    remember. Not established: allowed by the user, or made unasked.
    A second try under another name was refused, the refusal was
    recorded, and no file was made; a connection to an outside host,
    refused, was not made (the chat's report);
  - the first chat fetched the check chat's result from the app by
    itself (the chat's report).
  The user's words on the whole: that they could not tell what the
  process was, or what the setup had done.
- **The second sheet's address.** Given by the chat as a link; clicked,
  it opened in the app's built-in browser and **showed 404** — the
  page is in a private repository and that browser holds no GitHub
  sign-in (the chat's report; the cause is its reading). The user
  wanted their usual browser.

So, on a route that sets nothing in a file: a check was run though
nothing had been set, in a chat of its own, and it could not be
followed and itself left a file outside the folder; and the one thing
the route has for the user to set stood among the advice and was not
set. `setup/codex-macos.md` and the base (`guide.md` § 4, "No step
for its own sake"; `vault-procedures.md` § 1) were changed by this:
the first sheet stays, holds what the route has to set, and has a
check only of what was set.

## A command stopped in the sandbox: whether the agent asks (read, 2026-11-23)

Raised by a chat on a vault whose start-of-session pull stopped on
`.git/FETCH_HEAD`. Read on 2026-11-23 through a summarising fetch —
the tool's issue tracker (`openai/codex`), its repository, the docs,
two forum threads; nothing tried. What was seen is above: a chat that
did not ask ("The ChatGPT desktop app on macOS", 2026-11-19), chats
that asked ("The app as it starts").

- **Nothing read says the agent must ask.** The built-in instructions
  in the repository
  (`codex-rs/protocol/src/prompts/base_instructions/default.md` and
  two model prompts under `codex-rs/core/`, main branch) say only that
  a call can be escalated to the user; the docs' pages on approvals
  and the sandbox describe no second try after a failure. The tool's
  schema and source were not read.
- **Open reports of a Git command stopped on `.git` with no prompt**,
  none answered by a maintainer: a fetch stopped on `.git/FETCH_HEAD`
  and the chat ended without a request, under Approve for me (#61936;
  a commenter on 26.1117.41037); a commit stopped on the lock file and
  the request to leave the sandbox refused before any prompt (#63187,
  26.1117.41037); such a request refused before any prompt, not Git
  (#32563, from 26.707 on).
- **No report was found of the opposite** — a command that needs
  approval under Ask for approval running outside the sandbox unasked.
  The reports of a choice that does not reach a chat run the other
  way: Full access chosen, the session sandboxed (#63033, #63078).
- **What the reporters do**: Git by their own hand in a terminal
  (#30102, a comment; #54574); a separate CLI session on the same
  clone (a comment on #62975).
- **An instruction in `AGENTS.md`**: no account was found of one that
  tells the agent to ask and run again. One report has an `AGENTS.md`
  that authorized Git commands and was not followed — the agent asked
  all the same (#51641, the desktop app 26.1002); a commenter there:
  the file instructs the model and grants nothing.

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
passes inside the profile; a submodule or a worktree
nested in the folder; whether a `read` glob is in fact rejected, and
what the app does with a profile that fails to compile; whether Codex
can write its own `config.toml` in a chat on Ask for approval; whether
the app ever writes `config.toml` over an edit left open in an editor;
what
happens when Codex starts another agent's program from the shell; what
the Configuration screen shows once a profile is in the file, and
whether a switch touched there writes the older sandbox keys back; how
many approvals a setup takes in a folder that is not under version
control, before any profile exists; whether a user's paste of a whole
`config.toml` goes as the five-line paste did.
