# dev-management-private

公開リポジトリ `dev-management` の非公開の相方。**このリポジトリは公開しない。**

公開側には、ルール・Skill・ひな形だけを置く。ここには、特定のプロジェクトに結びついた材料と、読み手（ユーザー）に合わせた参照データを置く。

```
dev-management-private/
├── device-instructions.md   このデバイスで常に守ること（エージェントの個人設定から読み込む）
├── samples/                 各プロジェクトで採った構造と、標準のルールから変えた点・その理由
├── writing-loop/            文章改善ループ（事例集 records/ と表現の辞書 expressions.md）
└── dictionary/              概念の辞書（読み手が理解した言葉での説明）
```

置き場所は `~/dev-management-private/` に固定する。公開側の Skill と `device-instructions.md` が、この場所を前提に参照するため。

## PC 間の共有

GitHub の非公開リポジトリで共有する（作り方は公開側 `dev-management` の README「dev-management-private を作る」）。事例集と辞書は、どの PC のエージェントも同じものを読む必要があるため（自律レビューの読み手にも渡す）。

- **push してよいのは、ユーザーが許可した PC だけ**。会社の PC など、企業の情報が漏れる恐れのある PC からは push しない。push しない PC では pull して使う
- 許可したかどうかはユーザーの判断で、システムには定義されていない。だから**エージェントはこのリポジトリを自分からは push しない**。push はユーザーが指示したときだけ行う
- コミットはエージェントが行ってよい（事例・辞書を足したとき）

## デバイスごとの設定

エージェントの個人設定（どのプロジェクトでも毎回読まれるファイル）から `device-instructions.md` を読み込ませる。Skill は依頼の文面が説明文に合ったときにしか本文が読まれないが、個人設定は毎回読まれるので、Skill の名前を言わなくても事例集と辞書の入口が届く。

| エージェント | 設定 |
|---|---|
| Claude Code | `~/.claude/CLAUDE.md` に `@~/dev-management-private/device-instructions.md` の1行を書く（[Claude Code: Memory](https://code.claude.com/docs/en/memory)。個人設定からのホームディレクトリの読み込みは、確認なしで読まれる） |
| Copilot・Codex | 個人設定の置き場所を、使い始めるときに公式ドキュメントで確かめて書き足す |

会社のプロジェクトに関する内容をここに置いてよいかは、会社の規程に従う。置く場合も、コードや固有名は書かず、構造と判断の理由にとどめる。
