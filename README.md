# dev-management

AI エージェントと一緒に進める開発・研究（シミュレータ環境の構築、モデルの学習・検証、認識アルゴリズムの検証など）を、後から人も AI も辿れる形で管理するためのルールと Skill 集。

このリポジトリは公開リポジトリである。**会社や特定のプロジェクトの情報は置かない。** プロジェクトのサンプルや文章改善の記録は、別の非公開リポジトリ（`dev-management-private`）に置く。

## 中身

```
dev-management/
├── .claude-plugin/marketplace.json     Claude 用の目録（このリポジトリにあるプラグインの一覧）
└── plugins/dev-management/             プラグイン本体
    ├── .claude-plugin/plugin.json      プラグインの名前・版・説明
    ├── skills/
    │   ├── project-rules/              管理ルール（Git、trials/ のノード、INDEX、複数プロジェクト）
    │   │   └── templates/              新しいプロジェクトとノードのひな形
    │   ├── explainer-style/            解説文書の書き方（文章）
    │   ├── explainer-citation/         解説文書の書き方（出典）
    │   ├── explainer-structure/        解説文書の書き方（構成）
    │   ├── explainer-review/           解説文書を出す前の検査
    │   ├── research-explainer-style/   研究解説の書き方（文章）
    │   ├── research-citation/          研究解説の書き方（出典）
    │   └── research-structure/         研究解説の書き方（構成）
    └── agents/implementer.md           Claude Code 用の実装担当サブエージェント
```

`.claude-plugin/` の2つの JSON は Claude がプラグインとして読み込むための目印で、中身の Skill やひな形はすべて普通の Markdown ファイルである。Claude 以外の環境では、`skills/` 以下をファイルとして使う。

## 導入

### claude.ai・Claude Desktop・Cowork

1. **Customize > Plugins > Add > Add marketplace** で、このリポジトリ（`<owner>/dev-management`）を登録する
2. 登録したマーケットプレイスで **Sync automatically** を有効にする
3. `dev-management` プラグインを追加する

追加したプラグインはアカウントに保存され、同じアカウントでサインインした Claude Code でも使える。以前に個別に登録した explainer 系・research 系の Skill やプラグインがある場合は、同じ Skill が二重に読み込まれないよう無効にする。

参考: [Plugins（claude.ai / Cowork）](https://claude.com/docs/plugins/overview)

### Claude Code（claude.ai とは別のアカウントで使う場合）

```bash
claude plugin marketplace add <owner>/dev-management
claude plugin install dev-management@dev-management
```

Skill は `/dev-management:project-rules` のように、プラグイン名を前に付けて呼び出せる。

参考: [Claude Code: Plugins overview](https://code.claude.com/docs/en/plugins)

### GitHub Copilot（VS Code）

Copilot はプラグインの目録を使わず、Skill を個人用フォルダ `~/.copilot/skills/` から読む。リポジトリを clone して、`skills/` 以下をそこへコピーする。

Linux / macOS:

```bash
git clone https://github.com/<owner>/dev-management.git ~/dev-management
mkdir -p ~/.copilot/skills
cp -r ~/dev-management/plugins/dev-management/skills/* ~/.copilot/skills/
```

Windows（PowerShell）:

```powershell
git clone https://github.com/<owner>/dev-management.git $HOME\dev-management
New-Item -ItemType Directory -Force $HOME\.copilot\skills | Out-Null
Copy-Item -Recurse -Force $HOME\dev-management\plugins\dev-management\skills\* $HOME\.copilot\skills\
```

参考: [VS Code: Agent Skills](https://code.visualstudio.com/docs/agent-customization/agent-skills)

## 新しいプロジェクトを始める

`plugins/dev-management/skills/project-rules/templates/project/` の中身をプロジェクトのルートにコピーし、`AGENTS.md` の冒頭を埋める。詳しくは `project-rules/SKILL.md` の「新しいプロジェクトを作るとき」。

## 変更するとき

- Skill やルールを変えたら、`plugins/dev-management/.claude-plugin/plugin.json` の `version` を上げる（どの版がどの環境に入っているかを追えるようにするため）
- コミットメッセージには種別を付ける（`feat` / `fix` / `refactor` / `docs`）
- Skill の内容の見直しは、非公開リポジトリの文章改善ループで議論して決める

## ライセンス

MIT License（`LICENSE` を参照）。
