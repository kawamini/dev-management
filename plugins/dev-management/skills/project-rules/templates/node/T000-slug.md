---
id: T000
status: running          # running / valid / invalid / abandoned
code_commit:             # 実行前に git rev-parse --short HEAD の値を書く
config:                  # 例：configs/lora_r32.yaml
seed:
env:                     # 例："uv.lock @ <コミット ID>, Isaac Lab の版, GPU ドライバの版"
created: YYYY-MM-DD
# runs:                  # 既存ノードの実行を再利用するときだけ書く
#   - {condition: <条件名>, from: <ノード ID>, commit: <コミット ID>}
---

# T000 <探究の内容>

## 背景

<!-- なぜこの探究を始めたか。親ノードの結果・知見を [[T000-slug]] の形でリンクする -->

## 目的

<!-- 確かめたいこと。1つに絞る -->

## 方法

<!-- 何をどう変え、どう測ったか。親ノードの設定・手順を使うならリンクする -->

再現：

```bash

```

## 結果

<!-- 観測したこと。解釈は書かない -->

## 知見

<!-- 結果から言えることと、それが成り立つ範囲 -->
