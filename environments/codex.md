# Codex

What has been found here about Codex, dated and sourced: the docs as
read on 2026-11-09, a vault's setup on Windows (2026-11-09), trials in
the ChatGPT desktop app on macOS (2026-11-19); and a reference
configuration drawn from those trials, each line with its reason. A
reference, never a requirement: which settings and guardrails a user has
is theirs to decide with their agent (`guide.md`). Where a note and the docs differ,
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
  recursively: a `git add` or `git commit` needs an escalation.
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
  `.agents`, `.codex` and `.aws` read-only inside each writable root;
  the network off. So a session can read whatever the user can, and
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
  together.
- **Profiles through the CLI** (`codex sandbox -P … -c …`, on throwaway
  repositories): `:root` deny with `:minimal` read blocked reads and
  writes outside; `.git` writable with `.git/config`, `.git/hooks` and
  the hooks folder read-only let commits through and refused `git
  config`; a `github.com` allowlist (`features.network_proxy`) let an
  anonymous `ls-remote` through and refused another host; the macOS
  Keychain credential helper failed inside the sandbox (a private
  repository unreachable) and passed outside. A profile with the sync
  inside it would need these and a credential readable inside; untried
  in the app. A workspace admin can enforce profiles
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

## A reference configuration: the ChatGPT desktop app on macOS (2026-11-19)

One configuration that worked on the test bed above, for a user who
works in a shared vault through Codex and wants few prompts; each line
with its reason. A reference, not a requirement: the user decides each
point with their agent, and the docs for their version win. Personal
preferences (keyboard shortcuts, the send key) are left out.

- **Mode: Approve for me**, chosen in the composer for each chat — the
  composer overrides `config.toml`. A vault's routine needs an
  escalation at every pull, commit, push and `gh` call; under Ask for
  approval each is a prompt, and many prompts wear attention down
  (`guide.md` § 2.10); Auto-review answers them instead. Auto-review is
  a model, not a boundary: what must never happen goes in `forbidden`
  rules (below). Full access off: with the General tab's toggle off it
  is not in the menu.
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
  arbitrary live content" — the trade-off. No permission profile: under
  one, nothing left the sandbox and the vault's sync failed (above).
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
kept: `guide.md` § 2.12, § 2.13, § 3.8.

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
the user's, and whether it can loosen it; where its login is stored on
Windows; which shell a `Bash` hook sees on Windows (the hooks page does
not say).
