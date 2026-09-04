# AGENTS.md

このリポジトリでAI・Codex・Claude Code・その他エージェントがUI・プロトタイプを作成または修正する場合は、以下を必須とする。

## Agent Project Preflight

```text
Agent Project
├─ ProjectButler-Prototype  ← Primary
├─ rules
└─ shared
```

UI作業前に `rules` と `shared` が見えていることを確認する。

どちらかが見えない場合は実装を開始しない。`rules` / `shared` をこのrepoへコピー・submodule化・推測代替してはいけない。

製品実装中は `rules` と `shared` を参照専用として扱う。

Claude Codeではrepoルート `CLAUDE.md` が `@AGENTS.md` をimportしていることを入口条件とする。

## Codex Git Sync Before Work

Codexでは、設計資料・共通ルール・テンプレートを読む前にPrimary・`rules`・`shared` の3repoを安全に同期する。

各repoで:

1. `git status --short`
2. 差分を「保護すべき作業差分」と「既知の生成物」に分類する。
3. ソース、設計書、設定、ルール、テスト定義など、人が編集した可能性がある差分がある場合は停止して報告する。
4. build / dist / output / coverage / cache / 一時ZIPなど、生成元と用途が明確な既知の生成物だけがある場合は、それだけを理由に停止しない。
5. 生成物か判断できない未追跡ファイルがある場合は推測せず停止して報告する。
6. 安全に続行できるなら `git fetch origin`。
7. HEADと `origin/main` のahead / behindを確認。
8. main上でbehindのみなら `git pull --ff-only origin main`。
9. detached HEAD / Codex worktreeでbehindのみなら `git merge --ff-only origin/main`。
10. ahead / diverged / main以外で自動解消が必要なら停止して報告。
11. 同期後に再確認。

重要:

- `git status --short` に何か表示されたという事実だけで停止しない。
- 既知の生成物だけが残っている状態は、作業中ソースのdirty状態と同一視しない。
- Preflight通過のためだけに生成物を勝手に削除・退避・commitしない。

禁止: 保護すべき作業差分がある状態でpull、`git reset --hard`、強制checkout、自動rebase、force push、diverged状態の自動解消。

## Required Read Order

1. このrepoの `PROJECT.md` / `README.md` / 関連設計資料
2. `rules/AGENTS.md`
3. `rules/README.md`
4. `rules/cluewly-github-rules.md`
5. 使用中AIのsetup
6. `shared/AGENTS.md`
7. `shared/template/AGENTS.md`
8. `shared/template/PROJECT.md`
9. `shared/template/README.md`
10. `shared/template/docs/AI_PAGE_CREATION_CHECKLIST.md`
11. `shared/template/docs/AI_SAFE_EDITING_GUIDE.md`
12. `shared/template/docs/UI_COMPONENTS.md`
13. 対象HTML / CSS / JS

## UI / Prototype Rule

### 新規ページはtemplateをそのまま使う

新規ページは `shared/template` を参考にして似たものを作ってはいけない。

```text
設計要件を読む
↓
SIDE NAV / SIDE CONTENT / MAIN ONLY を選ぶ
↓
対応する shared/template/main-*.html をPrimary側へそのままコピー
↓
コピーしたHTMLを直接編集
↓
必要な共通CSS/JSもshared/templateから内容変更せずコピー
↓
templateにない不足部分だけページ固有実装
```

`main-*.html` をコピーせずに新しいDOMを組み立てることは禁止する。

### templateの既存ブロックを守る

仕様上変更が不要なtemplateブロックは削除・置換・転用しない。

仕様上変更が必要な箇所も、まず既存HTML構造とclassを残したまま内容を差し替える。

既存構造では要件を実現できない場合だけ、不足部分の追加またはブロック置換を行う。

### 個別UIもtemplateから使う

3 MAINはページ骨格の選択であり、個別UIの再利用元を制限しない。

選択templateに必要な部品がない場合は、他の `main-*.html` とPrimaryの完成済みUIを確認する。

優先順位:

```text
1. 選択したmain-*.htmlの完成済みUI
2. 他のmain-*.html / Primaryの完成済みUI
3. common.css等の共通class
4. templateでは表現できない不足部分だけ新規実装
```

既存完成UIがあるのに、別component / class / CSSを作って似せてはいけない。

### 共通CSS/JS

Primaryに承認済み共通層がない新規プロトでは、必要な `common.css` / `layout.css` / `sidebar.css` / `common.js` をshared/templateから内容変更せずコピーする。

Primaryに承認済み共通層がある場合はPrimary側を優先する。

Side Paneは `initResizableSidePane()` を使う。

### ページ固有CSS

ページ固有CSSはtemplateに存在しないページ固有要件だけに使う。

テンプレート由来UIの `height / min-height / padding / gap / font-size / icon size / radius / color / hover / active` を変えるためのCSSを追加しない。

### 禁止

- templateを見て似たDOMを新規作成
- template既存ブロックを仕様にない別用途へ転用
- 既存完成UIがあるのに似た新規componentを作成
- template値の近似再現
- 後勝ちselector
- 理由のない `!important`
- inline style
- fix/temp/override/patch用class
- 指示外ファイルの変更
- 製品実装の副作用として `rules` / `shared` を変更
- ProjectButler固有コードを他製品へ横展開

## 既存ページ修正

既存ページでは既存実装を優先して保持する。

shared/templateと異なることだけを理由に全面移行しない。

全面移行が必要なら理由と影響範囲を報告し、承認前に実行しない。

## Completion Guard

完了前に確認する。

- 新規ページで選択した `main-*.html` を実際にコピーしたか
- 仕様にないtemplateブロックを削除・転用していないか
- template全体から使える完成済みUIを確認したか
- 既存完成UIがあるのに似た新規UIを作っていないか
- template由来UIの寸法・状態をページ固有CSSで変更していないか
- コピーした共通CSS/JSがshared元と一致しているか
- completed HTMLを選択templateと比較したか

最低限:

```text
Primary: git status --short / git diff --check
rules:   git status --short / git diff --check
shared:  git status --short / git diff --check
```

`rules` / `shared` に意図しない変更がある状態で完了報告しない。
