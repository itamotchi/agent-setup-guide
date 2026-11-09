# agent-setup-guide

A guide by which a user and their coding agent build the user's own
**harness** — the settings and instructions that hold the agent on the
user's machine — for an agent that handles knowledge bases (folders of
documents under git, "vaults"). For any agent tool, Claude Code, Codex
or another, on any platform.

It says what to watch for and what the user decides; it does not say
what anyone's settings must be. The environment notes and the templates
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

## Use

1. **Get the folder.** On this page, Code → Download ZIP, and unzip it;
   or `git clone` it. Put it anywhere *outside* other projects and
   vaults: an agent opened on a folder also reads the instruction files
   of the folders above it.
2. **Open your agent on that folder**, in the mode where it asks before
   it writes (the default in most tools; not a mode that skips
   approvals). If it asks whether to trust the folder, you may: see
   above.
3. **Say:** "Read AGENTS.md in this folder and follow it: set up my
   agent's settings with me."
4. **Answer, one point at a time.** The agent asks in your language,
   says what it will write and why, backs up each settings file before
   a change, and writes only what you approve — outside this folder.
   Refuse any approval prompt you do not understand; the agent then
   explains it.
5. **Check.** At the end, in a new session opened on another folder, the
   agent tries what should be refused — reading a secret, running a
   denied command, writing outside the folder — and shows you each
   refusal.
6. **Report, if you like.** The agent writes a short report: what in the
   guide was unclear, wrong or missing, and what it found about your
   environment. You may post it as an issue on this repository — with
   no names, paths or secrets in it.

## Contents

| path | what it is |
|---|---|
| `guide.md` | the guide: what a harness is, the layers, what to watch for, what the user decides, how to proceed and check, the report back |
| `environments/` | notes on particular tools and platforms: Claude Code on macOS, Claude Code on native Windows, Codex |
| `model/` | templates for Claude Code: the user settings fragment, a rules file, a user memo; `windows/` for native Windows; `optional/` |

## License

CC0 1.0 Universal (`LICENSE`): copy and adapt freely.

## 日本語

ナレッジベース（git で管理する文書のフォルダ。vault）を扱うエージェント
について、ユーザとそのエージェントが、ユーザ自身のハーネス — エージェン
トを手元で律する設定と指示 — を組み立てるための手引きです。Claude Code、
Codex など、道具も OS も問いません。何に気をつけ、何をユーザが決めるか
を示すもので、設定の正解を示すものではありません。環境ごとのノートと雛
形は参考で、正は各ツールの公式ドキュメントです。下書きの段階です（2026
年11月）。

**このフォルダにあるのは文章と雛形だけです。** 設定ファイルも、自動で実
行されるもの（hook など）もありません。エージェントで開いても、何も実行
されません。

### いつ

ナレッジベースでエージェントを初めて開く前に。開くと、その vault の規則
とフックが動き、他の人が書いた文章（Issue やコメント）がセッションに届く
ことがあります。vault の規則が守るのは vault の中です。その外でエージェ
ントが何を読み・書き・実行してよいかは、あなたのハーネスが決めます。

### 使い方

1. **フォルダを手に入れる。** このページの「Code」→「Download ZIP」で取
   得して展開します（git を使える人は clone でも構いません）。置き場所は、
   他のプロジェクトや vault の外にしてください。エージェントは、開いたフ
   ォルダより上のフォルダにある指示ファイルも読むからです。
2. **そのフォルダでエージェントを開く。** 書き込む前に確認してくるモード
   で開きます（多くの道具では最初からそのモードです。確認を省くモードは
   使いません）。「このフォルダを信頼するか」と聞かれたら、信頼して構い
   ません（理由は上のとおりです）。
3. **こう伝える：**「このフォルダの AGENTS.md を読んで、それに沿って、私
   のエージェントの設定を一緒に決めてください。」
4. **一つずつ答える。** エージェントはあなたの言葉で尋ね、何をなぜ書くか
   を先に言い、設定ファイルは書き換える前に控えを取り、あなたが承諾した
   ものだけを、このフォルダの外に書きます。分からない確認画面は拒否して
   構いません。そのあとエージェントが何の確認だったかを説明します。
5. **確かめる。** 最後に、別のフォルダで新しいセッションを開き、拒まれる
   べき操作 — 秘密の読み取り、禁じたコマンド、フォルダの外への書き込み
   — が本当に拒まれるかを、エージェントが一つずつ試して見せます。
6. **報告する（任意）。** エージェントが、手引きの分かりにくい点・誤り・
   不足と、あなたの環境で分かったことを短い報告にまとめます。よければ、
   このリポジトリの Issue に投稿してください。名前、パス、秘密情報は含め
   ないでください。

ライセンスは CC0 1.0 です（自由に写して直せます）。
