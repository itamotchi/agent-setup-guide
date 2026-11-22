# GitHub: an agent's credential, and who owns the repository

What has been found here about GitHub where it bears on an agent's
harness: with which credential an agent reaches a vault's repository,
how far each kind reaches, who can make which, and what an organization
and a transfer of a repository change. Dated and sourced: GitHub's docs
as read on 2026-11-21 (docs.github.com; quotes are short, and a
sentence cut short is marked "…"), and a trial on GitHub the same day
with two accounts and one private repository. What was tried and what
was only read is marked. This note is the evidence; what a setup does
with it is in the files of `setup/`. Where this note and GitHub's docs
differ, the docs win.

**The finding that matters most**: a fine-grained personal access token
limited to one repository can be made by the repository's owner, or by
a member of the organization that owns it — **not by a collaborator on
a repository that another personal account owns** (the docs, and
tried). A vault whose users are collaborators on its owner's personal
repository cannot give each user such a token.

## The credentials, and how far each reaches

An agent that runs `git` and `gh` acts on GitHub with a credential of
the user's. Where the agent's commands run inside a sandbox that cannot
reach the operating system's keychain, the credential is a token kept
in a file the agent's commands can read (`environments/codex.md`, "A
credential inside"); so its reach is the agent's reach on GitHub.

- **A fine-grained personal access token.** "Each token is limited to
  access resources owned by a single user or organization." At its
  creation the user picks the resource owner, then the repositories —
  "Only select repositories" takes several, a maximum is not stated —
  and the permissions (Contents, Issues, and so on, each none, read, or
  read and write). "Tokens always include read-only access to all
  public repositories on GitHub." An organization can cap its lifetime
  (366 days by default). GitHub "recommends that you use fine-grained
  personal access tokens instead of personal access tokens (classic)…".
  Read; and tried for one repository with Contents and Issues read and
  write.
- **A personal access token (classic).** "Your personal access token
  (classic) can access every repository that you can access." The
  `repo` scope "Grants full access to public and private repositories…";
  no scope is limited to one repository. It need not expire. Read.
- **The user's own login** — the token `gh auth login` stores, used by
  `gh` and, through its helper, by Git. It acts as the user on whatever
  the user can reach, like a classic token (as it is known to work; not
  read in the docs for this note). A harness that lets `git
  push` or `gh` run outside the sandbox with this login (Claude Code on
  macOS, as set up here) gives the agent's permitted commands that
  reach, without handing it the token as a file.
- **A deploy key**: "an SSH key that grants access to a single
  repository", attached to the repository, not to an account; read-only
  unless write access is given; no expiry. SSH only: the docs describe
  no use of it for the API, so issues through `gh` are not served by
  it. Read.
- **A GitHub App's user access token**: "can only access resources that
  both the user and app can access"; it "expires after 8 hours", the
  refresh token after 6 months. It needs an app made and installed by
  the repository's owner and a program that runs the authorization and
  the refresh. Read; not tried, and not proposed here.

All personal access tokens "are tied to the user who generated them":
what a token does is recorded as that user's act. A token made by one
person and handed to another makes the other act as the first.

## Who can make a fine-grained token for a repository

The docs list, under "The major gaps in fine-grained personal access
tokens": "Using fine-grained personal access token to contribute to
repositories where the user is an outside or repository collaborator."
And: "Outside collaborators can only use personal access tokens
(classic) to access organization repositories…" (those they are a
collaborator on). The docs add that these gaps "are not permanent".

Tried, 2026-11-21, with a second account and a private repository, the
token's creation page looked at in each position:

| the second account is | the repository at "Only select repositories" |
|---|---|
| a collaborator (invitation accepted) on the repository, owned by another personal account | not offered |
| an outside collaborator, after the repository was transferred to an organization | not offered: the organization is not listed under "Resource owner" |
| a member of that organization | offered: the organization is listed under "Resource owner", the repository can be selected |

The member's token, once approved by the organization's owner (below),
read the private repository and created an issue through `gh` (`gh api
repos/<owner>/<repository>`, and a POST to its `/issues`).

So, for a vault's repository:

- owned by the user: the user makes the token;
- owned by an organization: each member makes their own, for that
  repository alone;
- owned by another person's personal account: no fine-grained token for
  a collaborator. What is left there: a classic token, which reaches
  everything that user can reach on GitHub — for a user whose account
  reaches nothing else of value the difference is small, for one whose
  account holds other work it is large; or no credential inside the
  sandbox, the user running the pull, the push and the issues by hand.

## An organization

- **What it is.** "You cannot sign in to an organization. Instead, each
  person signs in to their user account…". It is made from a signed-in
  personal account (Settings, Organizations, "New organization", then
  the prompts — which ask whether it belongs to the personal account or
  to a business or institution, and for the latter its name: seen). The
  other route, converting a personal account into an organization,
  ends that account's sign-in and cannot be undone (read).
- **The plan.** "You can use organizations for free, with GitHub Free,
  which includes limited features on private repositories." What the
  paid Team plan adds in private repositories: protected branches, code
  owners, required reviewers, wikis, Pages (read).
- **Base permissions.** The setting stood at **Read** in a newly made
  organization (seen): every member can read every repository of the
  organization. An owner who wants members to see only what each is
  given sets it to none and grants a role on each repository; "Base
  permissions do not apply to outside collaborators" (read).
- **The approval of fine-grained tokens.** "Require administrator
  approval" is the default: a member's token is "pending until it is
  reviewed by an organization administrator", and "Your token will only
  be able to read public resources until it is approved" (read). Seen: the token was marked pending
  at its creation; the owner approved it under the organization's
  settings (Personal access tokens, Pending requests); a token that was
  regenerated was pending again and needed a second approval. The
  setting can be turned off ("Tokens requested for this organization
  will work immediately…": the screen's own words; not tried). While a
  token waits, "GitHub will notify organization owners with a daily
  email" of the tokens awaiting approval, and the token's maker is
  mailed when it is approved or denied (read 2026-11-22). What
  the approval gives is the owner's look at each token before it works;
  it is not what bounds the token — a token reaches no more than the
  member can reach, so the bound is the access each member is given.
  "Organization owners can view all fine-grained personal access tokens
  that can access resources owned by the organization" and revoke them;
  classic tokens are not shown there (read).
- **Classic tokens** can be kept from the organization's resources by a
  setting of the same page (read; not tried).
- **Other apps.** "When you create a new organization, OAuth app access
  restrictions are enabled by default": an OAuth app the owner has not
  approved has no API access to the organization's private resources.
  GitHub CLI, GitHub Desktop and Git Credential Manager are "privileged"
  apps: "users can still authorize privileged OAuth apps and use them
  to access data from the organization" (read). Tried: the owner's ordinary `gh` login
  read and wrote a private repository of a newly made organization with
  nothing approved. A GitHub App is installed on an account or an
  organization, each separately (read): an integration that reached the
  repository under its former owner has to be approved or installed
  again in the organization.

## A transfer of a repository to an organization

- **What moves** (read): "its issues, pull requests, wiki, stars, and
  watchers are also transferred"; webhooks, secrets and deploy keys
  stay attached. Needed: admin access to the repository and the right
  to create one in the organization.
- **The people** (read, and seen): "Other collaborators to the
  transferred repository remain intact" — as outside collaborators of
  the organization, who cannot make a fine-grained token (above). To
  make one, each is invited to the organization as a member.
- **The old address.** "All links to the previous repository location
  are automatically redirected to the new location", and `git clone`,
  `git fetch`, `git push` are redirected, until a repository is made at
  the old place; "we strongly recommend updating any existing local
  clones" (`git remote set-url origin <new address>`) (read). Tried,
  after a transfer, with the former owner's ordinary login and the old
  owner's name: `git ls-remote` over HTTPS passed; `gh api` read the
  repository — the reply named it at its new place — and a POST created
  an issue. So a vault's routine keeps running on
  the old address right after a transfer; the address is still to be
  changed, in each clone and wherever a vault's files name it. Changing
  a clone's address writes its `.git/config`, which a harness may keep
  from the agent: then it is the user's command.
- **A token made before the transfer.** A fine-grained token names one
  resource owner; one made with the former owner as resource owner
  should stop reaching the repository once the organization owns it,
  and be made again with the organization as owner. Inferred from the
  docs; not tried.
- **The plan**: "If you transfer a private repository to a GitHub Free
  user or organization account, the repository will lose access to
  features like protected branches and GitHub Pages" (read).

## A link that fills in the token's page

Read 2026-11-22 (GitHub's docs, "Managing your personal access
tokens"); not tried here. "You can share templates for a fine-grained
personal access token via links." The parameters, all optional: `name`
(up to 40 characters), `description`, `target_name` ("User or
organization slug"; the signed-in user if absent), `expires_in` (1 to
366, or `none`), and a permission by its name with `read`, `write` or
`admin` — `contents`, `issues`, `metadata` among them; "`write` always
includes `read`". So, for a vault's repository in an organization:

```
https://github.com/settings/personal-access-tokens/new?name=<name>&target_name=<organization>&expires_in=<days>&contents=write&issues=write&metadata=read
```

No parameter sets the repositories: the user still chooses "Only select
repositories" and the repository, then generates and copies.

## Handing the token to the command that stores it

- **The token's string is shown once**, on the page that follows its
  creation — also while the token is pending approval. Seen: the page
  was left for the approval and the string could not be shown again;
  "Regenerate token" gives a new one, which is pending again. So the
  string is stored at once, before any approval.
- **A paste that shows nothing invites a second paste.** Tried with a
  shell's hidden input (`read -s`): nothing appears, and the user's
  doubt is whether it went in; pasted twice, the string is wrong with
  no sign. A check that does not show the string is its length (a
  fine-grained token was 93 characters).
- **From the clipboard** (`pbpaste` on macOS) there is no paste, but
  the order matters: copying the command itself replaces the token on
  the clipboard. What worked: the command put into the terminal first,
  Enter not pressed; the token copied with the page's copy button; then
  Enter.

## Not tried

- Git over HTTPS — a pull, a push — with a member's fine-grained token.
  Seen 2026-11-22: `git ls-remote` on the private repository passed
  with a member's token, approved by the owner, inside an agent's
  sandbox (`environments/codex.md`, "A setup followed from its first
  step"); on another day the owner's own fine-grained token passed
  `git ls-remote`, a pull and a push dry run.
- The link that fills in the token's page (above).
- That a member's token is refused at a repository of the same
  organization that was not selected.
- Whether `gh auth login --with-token` accepts a token still pending
  approval, and stores it.
- The approval turned off; classic tokens shut out of an organization;
  an OAuth app or a GitHub App approved or installed in one.
- A token made before a transfer, used after it.
- More than one pair of accounts, one repository, one day; GitHub
  changes, and its docs call the gaps above temporary.
