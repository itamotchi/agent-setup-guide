# agent-setup-guide

A guide by which a user and their coding agent build the user's own
**harness** — the settings and instructions that hold the agent on the
user's machine — for an agent that handles knowledge bases (folders of
documents under git, "vaults"). For any agent tool, Claude Code, Codex
or another, on any platform.

It says what to watch for and what the user decides; it does not say
what anyone's settings must be. The environment notes and the templates
are references: partial, dated, never a requirement. The tool's own
official documentation is the source of truth.

## Use

1. Clone this repository.
2. Open your agent on the cloned folder. An agent that reads `AGENTS.md`
   on opening a folder is pointed to `guide.md`; otherwise, ask it to
   read `guide.md` and follow it.
3. The agent talks with you in your language, one point at a time, and
   writes your settings only on your yes — outside this folder.
4. At the end it writes a short report: what in the guide was unclear,
   wrong or missing, and what it found about your environment
   (`guide.md` § 5).

## Contents

| path | what it is |
|---|---|
| `guide.md` | the guide: what a harness is, the layers, what to watch for, what the user decides, how to proceed and check, the report back |
| `environments/` | notes on particular tools and platforms: Claude Code on macOS, Claude Code on native Windows, Codex |
| `model/` | templates for Claude Code: the user settings fragment, a rules file, a user memo; `windows/` for native Windows; `optional/` |

## Status

Drafted in November 2026 from one user's setup of Claude Code on macOS.
The first trial on another environment, Codex on Windows, is pending.

## License

CC0 1.0 Universal (`LICENSE`): copy and adapt freely.

## 日本語の要約

ナレッジベース（git で管理する文書のフォルダ）を扱うエージェントについて、
ユーザとそのエージェントが、ユーザ自身のハーネス（エージェントを手元で律
する設定と指示）を組み立てるための手引きです。Claude Code、Codex など道
具も OS も問いません。何を気にかけ、何をユーザが決めるかを示すもので、設
定の正解を示すものではありません。環境ごとのノートと雛形は参考で、正は各
ツールの公式ドキュメントです。

使い方：このリポジトリを clone し、そのフォルダでエージェントを開きます。
エージェントは `AGENTS.md` から `guide.md` に進み、あなたの言葉で一つず
つ尋ねながら進めます。設定は、あなたが承諾したものだけが、このフォルダの
外に書かれます。ライセンスは CC0 1.0 です。
