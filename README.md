# agent-setup-guide

A guide by which a user and their coding agent build the user's own
**harness** — the settings and instructions that hold the agent on the
user's machine — for an agent that handles knowledge bases (folders of
documents under git, "vaults"). For any agent tool, Claude Code, Codex
or another, on any platform.

It says how to judge a setting, what to watch for and what the user
decides; it does not say what anyone's settings must be. The way it
judges: start from the tool's standard configuration, and add a line of
your own only where a problem actually arose, or where the standard is
known to leave a harm that is silent and cannot be undone.

It works for any environment: your agent builds the harness for the
tool it is, from this guide and the tool's own documentation. For two
environments there is also a record of a setup that worked — what was
set, in order, why, and how far it was tried: Codex in the ChatGPT
desktop app on macOS, and Claude Code on macOS (`setup/`). Your agent
consults it and checks it against the tool as it is today; tools change
within weeks. These records, the environment notes and the templates
are references: partial, dated, never a requirement. The tool's own
official documentation is the source of truth. It is a draft (November
2026).

**This folder holds text and templates only** — no settings file, no
hook, nothing that runs. Opening it in your agent runs nothing.

## When

Before you first open your agent on a knowledge base. Once you do, the
vault's own rules and hooks run, and text written by others — issues,
comments — may reach the session. A vault's rules guard the vault; what
your agent may read, write and run outside it is your harness's to
decide.

And again later, whenever something needs changing: a prompt that keeps
coming, a tool your work needs that the harness stops, an update. Most
settings of your own are added then, not on the first day (`guide.md`
§ 4, "Later").

## Use

If you were invited to a shared vault, start from that vault's
introductory document: it gives you a text to paste to your agent, with
what that vault needs. Otherwise:

1. **Get the folder.** On this page, Code → Download ZIP, and unzip it;
   or `git clone` it. Keep it as a folder of its own for setting up,
   *outside* other projects and vaults: an agent opened on a folder also
   reads the instruction files of the folders above it.
2. **Open your agent on that folder**, in the mode where it asks before
   it writes (the default in most tools; not a mode that skips
   approvals). If it asks whether to trust the folder, you may: see
   above. With Codex, a folder downloaded as a ZIP opens read-only and a
   clone opens writable; either will do, since the agent writes nothing
   in this folder.
3. **Say:** "Read AGENTS.md in this folder and follow it: set up my
   agent's settings with me."
4. **Leave it to the agent, or answer one point at a time.** The agent
   offers both, and says first how far the set it proposes was tried.
   If you leave it, it applies that set, asks you only what you alone
   know, and tells you afterwards what it set. Either way it asks in
   your language, says what it will write and why, backs up each
   settings file before a change, and writes only what you approve —
   outside this folder. The application's own settings come first, and
   only a few are needed: the agent names a switch and what it is for,
   and you set it if you agree. Some files you
   paste yourself, where the tool does not let the agent write its own
   settings. Refuse any approval prompt you do not understand; the agent
   then explains it.
5. **Check.** At the end, in a new session, the agent tries what should
   be refused — reading a secret, running a denied command, writing
   outside the folder — and shows you each refusal. Only then is a vault
   cloned and opened.
6. **Report, if you like.** The agent writes a short report: what in the
   guide was unclear, wrong or missing, and what it found about your
   environment. You may post it as an issue on this repository — with
   no names, paths or secrets in it.

## Contents

| path | what it is |
|---|---|
| `guide.md` | the guide, and the procedure, for any tool: what a harness is, the layers, what to watch for, what the user decides, the steps of a setup and of a later change, the report back |
| `setup/` | a worked route for one tool and platform, dated — what was set, in order, each value with its reason, how far it was tried: Codex in the ChatGPT desktop app on macOS, Claude Code on macOS. A reference to consult and check, not a procedure to apply unread |
| `environments/` | the dated findings those routes rest on: Claude Code on macOS, Claude Code on native Windows (from the docs alone), Codex, GitHub |
| `model/` | the text of the templates for Claude Code: the user settings fragment, a rules file, a user memo; `windows/` for native Windows; `optional/` |
| `vault-procedures.md` | for a vault's agent: how a vault writes its own procedure for its users so that it holds for any environment — what the vault states of its needs, how a user reaches the procedure, a later change, adding a git repository inside the vault |

## License

CC0 1.0 Universal (`LICENSE`): copy and adapt freely.

## 日本語

ナレッジベース（git で管理する文書のフォルダ。vault）を扱うエージェント
について、ユーザとそのエージェントが、ユーザ自身のハーネス — エージェン
トを手元で律する設定と指示 — を組み立てるための手引きです。Claude Code、
Codex など、道具も OS も問いません。何に気をつけ、何をユーザが決めるか
を示すもので、設定の正解を示すものではありません。判断のしかたは一つで
す：道具の標準の構成から始め、自分の設定を足すのは、実際に問題が起きた
とき、または標準のままでは「気づかないうちに起き、取り返しのつかない
害」が残ると分かっているときだけにします。

どの環境でも使えます。あなたのエージェントが、この手引きと道具の公式
ドキュメントから、その道具に合ったハーネスを組み立てます。二つの環境に
ついては、実際にうまくいった設定の記録も置いています。何を、どの順で、
なぜ設定したか、どこまで試したか、です：macOS の ChatGPT デスクトップア
プリの Codex と、macOS の Claude Code（`setup/`）。エージェントはそれを参
考にし、いまの道具と照らして確かめます。道具は数週間で変わるからです。
この記録、環境ごとのノート、雛形はいずれも参考で、正は各ツールの公式ド
キュメントです。下書きの段階です（2026年11月）。

**このフォルダにあるのは文章と雛形だけです。** 設定ファイルも、自動で実
行されるもの（hook など）もありません。エージェントで開いても、何も実行
されません。

### いつ

ナレッジベースでエージェントを初めて開く前に。開くと、その vault の規則
とフックが動き、他の人が書いた文章（Issue やコメント）がセッションに届く
ことがあります。vault の規則が守るのは vault の中です。その外でエージェ
ントが何を読み・書き・実行してよいかは、あなたのハーネスが決めます。

そして、あとで何かを変えたくなったときにも使います。同じ確認が何度も出
る、仕事に要る道具がハーネスに止められる、道具が更新された、などです。
自分の設定の多くは、初日ではなくそのときに足されます（`guide.md` § 4
「Later」）。

### 使い方

共有の vault に招待された方は、その vault の案内文書から始めてください。
エージェントに貼り付ける文面が、その vault に必要な事柄とともに用意され
ています。それ以外の方は：

1. **フォルダを手に入れる。** このページの「Code」→「Download ZIP」で取
   得して展開します（git を使える人は clone でも構いません）。設定作業用
   の独立したフォルダとして、他のプロジェクトや vault の外に置いてくださ
   い。エージェントは、開いたフォルダより上のフォルダにある指示ファイル
   も読むからです。
2. **そのフォルダでエージェントを開く。** 書き込む前に確認してくるモード
   で開きます（多くの道具では最初からそのモードです。確認を省くモードは
   使いません）。「このフォルダを信頼するか」と聞かれたら、信頼して構い
   ません（理由は上のとおりです）。Codex では、ZIP で取得したフォルダは
   読み取り専用で、clone したフォルダは書き込み可で開きますが、どちらで
   も構いません。エージェントはこのフォルダには何も書かないからです。
3. **こう伝える：**「このフォルダの AGENTS.md を読んで、それに沿って、私
   のエージェントの設定を一緒に決めてください。」
4. **おまかせにするか、一つずつ答える。** エージェントは最初に両方を示
   し、推す一式がどこまで実際に試されたものかを先に伝えます。おまかせに
   すると、その一式をそのまま入れ、あなたにしか分からないことだけを尋
   ね、入れたあとで何を設定したかを伝えます。どちらの場合も、エージェン
   トはあなたの言葉で尋ね、何をなぜ書くかを先に言い、設定ファイルは書き
   換える前に控えを取り、あなたが承諾したものだけを、このフォルダの外に
   書きます。最初はアプリ自体の設定で、必要なものはわずかです。エージェ
   ントが項目とその目的を示し、納得したらあなたが切り替えます。道具がエ
   ージェントに自分の設定を書かせない場合は、あなた自身がファイルに貼り
   付けます。分からない確認画面は拒否して構いません。そのあとエージェン
   トが何の確認だったかを説明します。
5. **確かめる。** 最後に、新しいセッションで、拒まれるべき操作 — 秘密
   の読み取り、禁じたコマンド、フォルダの外への書き込み — が本当に拒ま
   れるかを、エージェントが一つずつ試して見せます。vault を clone して開
   くのは、そのあとです。
6. **報告する（任意）。** エージェントが、手引きの分かりにくい点・誤り・
   不足と、あなたの環境で分かったことを短い報告にまとめます。よければ、
   このリポジトリの Issue に投稿してください。名前、パス、秘密情報は含め
   ないでください。

ライセンスは CC0 1.0 です（自由に写して直せます）。
