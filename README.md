# dev-management

AI エージェントと一緒に進める開発・研究（シミュレータ環境の構築、モデルの学習・検証、認識アルゴリズムの検証など）を、後から人も AI も辿れる形で管理するためのルールと Skill 集。

このリポジトリは公開リポジトリである。**会社や特定のプロジェクトの情報は置かない。** プロジェクトのサンプル、文章改善の事例集、読み手に合わせた辞書は、別の非公開リポジトリ（`dev-management-private`）に置く。

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

1. **Customize > Plugins > Add > Add marketplace** で、このリポジトリ（`kawamini/dev-management`）を登録する
2. 登録したマーケットプレイスで **Sync automatically** を有効にする
3. `dev-management` プラグインを追加する

追加したプラグインはアカウントに保存され、同じアカウントでサインインした Claude Code でも使える。

以前に explainer 系・research 系の Skill を1つずつアップロードしていた場合は、プラグインの Skill（`dev-management:explainer-style` など）が一覧に出たことを確かめてから、アップロードした方をオフにする（削除ではなく、Skill の一覧の切り替えで無効にする）。両方が有効だと同じ Skill が2つ並び、どちらが読み込まれるか決まらないうえ、アップロードした方はプラグインを更新しても古いまま残る。先にオフにすると、プラグインが入るまでの間は Skill が1つも効かなくなる。

参考: [Plugins（claude.ai / Cowork）](https://claude.com/docs/plugins/overview)

### Claude Code（claude.ai とは別のアカウントで使う場合）

```bash
claude plugin marketplace add kawamini/dev-management
claude plugin install dev-management@dev-management
```

Skill は `/dev-management:project-rules` のように、プラグイン名を前に付けて呼び出せる。

参考: [Claude Code: Plugins overview](https://code.claude.com/docs/en/plugins)

### GitHub Copilot（VS Code）・OpenAI Codex

Copilot と Codex はプラグインの目録を使わず、Skill を個人用フォルダから読む。`~/.agents/skills/` は両方が読む場所なので、ここに置けば1か所で済む。Copilot の中で GPT や Claude などモデルを切り替えても、Skill と `AGENTS.md` を読み込むのは Copilot なので、同じように届く。

リポジトリを clone し、各 Skill のフォルダへのリンク（Linux・macOS はシンボリックリンク、Windows はジャンクション）を `~/.agents/skills/` に置く。コピーではなくリンクにするのは、`git pull` だけで更新が届くようにするためである。

Linux / macOS:

```bash
git clone https://github.com/kawamini/dev-management.git ~/dev-management
mkdir -p ~/.agents/skills
for d in ~/dev-management/plugins/dev-management/skills/*/; do
  ln -sfn "${d%/}" ~/.agents/skills/"$(basename "$d")"
done
```

Windows（PowerShell。ジャンクションは管理者権限なしで作れる）:

```powershell
git clone https://github.com/kawamini/dev-management.git $HOME\dev-management
New-Item -ItemType Directory -Force $HOME\.agents\skills | Out-Null
Get-ChildItem -Directory $HOME\dev-management\plugins\dev-management\skills | ForEach-Object {
  $link = Join-Path $HOME\.agents\skills $_.Name
  if (-not (Test-Path $link)) { New-Item -ItemType Junction -Path $link -Target $_.FullName | Out-Null }
}
```

更新は `git -C ~/dev-management pull` だけでよい。Skill を新しく足したときは、上のリンクの作成をもう一度実行する。リンクのままで Skill が読み込まれない場合は、リンクの代わりに各フォルダをコピーする（その場合、更新のたびにコピーし直す）。

Claude Code は `~/.agents/skills/` を読まないので、同じ PC でプラグインと併用しても Skill は二重にならない。

参考: [VS Code: Agent Skills](https://code.visualstudio.com/docs/agent-customization/agent-skills)、[Codex: Build skills](https://learn.chatgpt.com/docs/build-skills)

### 導入できたかを確かめる

| 環境 | 確かめること |
|---|---|
| Claude Code | Skill の一覧に `dev-management:project-rules` があり、同じ名前の Skill が2つ並んでいない。サブエージェントの一覧（`/agents`）に `implementer` がある |
| Copilot・Codex | `~/.agents/skills/` に Skill が並んでいて、チャットで project-rules を使う依頼をすると Skill として読み込まれる |

導入されていない環境では、エージェントは `AGENTS.md` の指示に従って GitHub 上のファイルを直接読み、Skill が導入されていないことをユーザーに伝える（`project-rules` の7.1）。

### dev-management-private を作る

`dev-management-private` は、サンプル・文章改善の事例集・表現の辞書・概念の辞書を置く非公開のリポジトリで、GitHub の非公開リポジトリを通して PC 間で共有する。事例集と辞書は、どの PC のエージェントも同じものを読む必要があるため。**push してよいのはユーザーが許可した PC だけ**で、会社の PC など企業の情報が漏れる恐れのある PC からは push しない（pull して使う）。許可したかどうかはシステムには定義されていないので、エージェントは自分からは push しない。中身の無いひな形を `plugins/dev-management/skills/project-rules/templates/private/` に置いてある。最初の1台はそこから作って GitHub の非公開リポジトリに push し、2台目以降はそれを clone する。

作るのは ① で、そのあと、その PC で使うエージェントに合わせて ② か ③（両方使うなら両方）を行う。

| 手順 | すること | 対象 |
|---|---|---|
| ① | 最初の1台：ひな形をコピーして Git リポジトリにし、GitHub の非公開リポジトリをリモートにする。2台目以降：clone する | すべての PC |
| ② | Claude Code の個人設定 `~/.claude/CLAUDE.md` に、`device-instructions.md` を読み込む1行を足す | Claude Code を使う PC |
| ③ | Copilot の個人設定 `~/.copilot/copilot-instructions.md` に、`device-instructions.md` の中身を書き足す | GitHub Copilot を使う PC |

`device-instructions.md` には、文章改善ループと辞書の入口（止まった質問を受けたら `writing-loop/README.md` に従う、説明を書く前に辞書を引く、など）が書いてある。Skill の本文は依頼が説明文に合ったときにしか読まれないが、個人設定は毎回読まれるので、ここに書いたことは毎回届く。②と③のどちらも、既存の個人設定は上書きせず、書き足すだけにしている。2回実行しても重ならない。

Linux / macOS:

```bash
# ① 2台目以降：非公開リポジトリを clone する（すでにある場合は何もしない）
#    <URL> は GitHub の非公開リポジトリ。最初の1台は、ひな形をコピーして git init し、remote add して push する
[ -e ~/dev-management-private ] || git clone <URL> ~/dev-management-private

# ② Claude Code：読み込みの1行を足す（同じ行が無いときだけ）
mkdir -p ~/.claude
line='@~/dev-management-private/device-instructions.md'
grep -qxF "$line" ~/.claude/CLAUDE.md 2>/dev/null || echo "$line" >> ~/.claude/CLAUDE.md

# ③ Copilot：中身を目印つきで書き足す（目印が無いときだけ）
f=~/.copilot/copilot-instructions.md
mkdir -p ~/.copilot
grep -qF 'dev-management-private:start' "$f" 2>/dev/null || {
  printf '\n<!-- dev-management-private:start -->\n'
  cat ~/dev-management-private/device-instructions.md
  printf '<!-- dev-management-private:end -->\n'
} >> "$f"
```

Windows（PowerShell）:

```powershell
# ① 2台目以降：非公開リポジトリを clone する（すでにある場合は何もしない）
$p = "$HOME\dev-management-private"
if (-not (Test-Path $p)) { git clone <URL> $p }

# ③ Copilot：中身を目印つきで書き足す（目印が無いときだけ）
$f = "$HOME\.copilot\copilot-instructions.md"
New-Item -ItemType Directory -Force (Split-Path $f) | Out-Null
if (-not ((Test-Path $f) -and (Select-String -Path $f -SimpleMatch 'dev-management-private:start' -Quiet))) {
  $src = Get-Content -Raw -Encoding utf8 "$p\device-instructions.md"
  Add-Content -Path $f -Encoding utf8 -Value "`n<!-- dev-management-private:start -->`n$src<!-- dev-management-private:end -->"
}
```

- ①：置き場所は `~/dev-management-private/` に固定する。公開側の Skill と `device-instructions.md` が、この場所を前提に参照するため
- ②：Claude Code は `@` で別のファイルを読み込めるので、1行だけ書く。`device-instructions.md` を直せば、そのまま反映される。効くのは、次に Claude Code を起動したセッションから（[Claude Code: Memory](https://code.claude.com/docs/en/memory)）
- ③：Copilot の個人設定には、`@` で別のファイルを読み込む書き方が無いので、中身をそのまま書き足す（[VS Code: Custom instructions](https://code.visualstudio.com/docs/agent-customization/custom-instructions)）。そのため `device-instructions.md` を直したときは、`<!-- dev-management-private:start -->` から `end` までを消して③を実行し直す。会社の PC などで、すでに個人設定に別の指示が書いてあっても、そのまま残る
- ③が実際に毎回読まれているかは、Copilot のチャットで「個人設定の指示に何が書いてあるか」を尋ねて確かめる（⚠️ 未検証：この PC には Copilot が無いので試していない）。Copilot のエージェントが、ワークスペースの外にある `~/dev-management-private/` のファイルを読むときは、確認を求められることがある
- Windows の②（`@~/...` の書き方が Windows の Claude Code で使えるか）は確かめていない
- Codex の個人設定は、使い始めるときに確かめて書き足す
- ひな形は、公開側を `pull` しても、作った後の `dev-management-private` には反映されない。ひな形が変わったときは、差分を見て手で取り込む
- 会社の PC など push しない PC は、**読むだけ**にする（事例集・辞書に足さない）。①の clone には、このリポジトリだけを読める fine-grained トークン（GitHub の Settings → Developer settings → Fine-grained tokens。Repository access はこのリポジトリだけ、Permissions は Contents の Read-only）を使う。アカウント全体の権限を置かずに済み、push も仕組みとして失敗する。clone のあと `touch ~/dev-management-private/.read-only` で印を置く。エージェントはこの印の有無で、事例集・辞書に足すかを判断する。個人アカウントのトークンを会社の PC に置いてよいかは、会社の規程に従う

## 新しいプロジェクトを始める

`plugins/dev-management/skills/project-rules/templates/project/` の中身をプロジェクトのルートにコピーし、`AGENTS.md` の冒頭を埋める。詳しくは `project-rules/SKILL.md` の「新しいプロジェクトを作るとき」。

## 変更するとき

- Skill やルールを変えたら、`plugins/dev-management/.claude-plugin/plugin.json` の `version` を上げる（どの版がどの環境に入っているかを追えるようにするため）
- コミットメッセージには種別を付ける（`feat` / `fix` / `refactor` / `docs`）
- Skill の内容の見直しは、非公開リポジトリの文章改善ループで議論して決める

## ライセンス

MIT License（`LICENSE` を参照）。
