# <プロジェクト名>

## このプロジェクトについて

- 目的：
- 外に提供するもの（他のプロジェクトから使われる場合。関数名・使い方・対応する版を数行で）：

## 管理ルール

このプロジェクトは `project-rules` Skill（dev-management）に従う。
Skill が使えない環境では、https://github.com/<owner>/dev-management の `plugins/dev-management/skills/project-rules/SKILL.md` を読む。

常に守ること：

- 作業を始める前に `trials/INDEX.md` を読む
- 実験を実行する前に `git status --porcelain` が空であることを確かめ、`git rev-parse --short HEAD` の値をノードの `code_commit` に書く
- `trials/` のノードのフォルダは移動・削除しない。誤りは `status: invalid` で表す
- `trials/*/outputs/` は Git に入れない
- 実装変更と記録の更新は別のコミットにする
