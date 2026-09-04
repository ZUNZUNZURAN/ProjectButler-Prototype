# AGENTS.md

このリポジトリでAI・Codex・Claude Code・その他エージェントがUI・プロトタイプを作成または修正する場合は、実装開始前に必ず以下を確認する。

## Agent Project Preflight

標準構成:

```text
Agent Project
├─ ProjectButler-Prototype  ← Primary
├─ rules
└─ shared
```

UI・プロトタイプ作業前に、source folder / additional directory `rules` と `shared` が見えていることを確認する。

どちらかが見えない場合は実装を開始せず、不足を報告する。`rules` / `shared` をこのrepoへコピー・submodule化・推測代替してはいけない。

製品実装中は `rules` と `shared` を参照専用として扱う。共通ルール・テンプレート・アイコン自体の変更を明示された場合だけ変更する。

Claude Codeではrepoルート `CLAUDE.md` が `@AGENTS.md` をimportしていることを入口条件とする。

## Codex Git Sync Before Work

Codexで作業する場合は、設計資料・共通ルール・テンプレートを読む前に、Primary・`rules`・`shared` の3repoを安全に同期する。

各repoで:

1. `git status --short`
2. dirtyなら自動同期せず、変更内容を報告して実装開始前に停止する。
3. cleanなら `git fetch origin`。
4. HEADと `origin/main` のahead / behindを確認する。
5. `main` 上でbehindのみなら `git pull --ff-only origin main`。
6. detached HEAD / Codex worktreeでbehindのみなら `git merge --ff-only origin/main`。既存の通常main worktreeをcheckoutで奪わない。
7. ahead / diverged / main以外のbranchで同期が必要なら、自動merge・rebase・resetを行わず状態を報告して停止する。
8. 同期後に状態を再確認する。

禁止: dirty状態でのpull、`git reset --hard`、強制checkout、自動rebase、force push、diverged状態の自動解消。

3repoすべてが安全に同期済みであることを確認してから、Required Read Orderへ進む。

## Required Read Order

1. このリポジトリの `PROJECT.md` / `README.md` / 関連設計資料
2. `rules/AGENTS.md`
3. `rules/README.md`
4. `rules/cluewly-github-rules.md`
5. 使用中のAIに対応するsetup
   - Codex: `rules/codex-project-setup.md`
   - Claude Code: `rules/claude-project-setup.md`
6. `shared/AGENTS.md`
7. `shared/template/AGENTS.md`
8. `shared/template/PROJECT.md`
9. `shared/template/README.md`
10. `shared/template/docs/AI_PAGE_CREATION_CHECKLIST.md`
11. `shared/template/docs/AI_SAFE_EDITING_GUIDE.md`
12. 対象HTML / CSS / JS

## UI / Prototype Rule

- 新規ページは `SIDE NAV` / `SIDE CONTENT` / `MAIN ONLY` の3つから適切なものを先に選ぶ。
- 選択は見た目ではなく左側領域の役割で判断する。
- 既存フォント・カラー・余白・角丸・シャドウ・ボタン・フォーム・パネルなど、`shared/template` の既存基本スタイルを優先する。
- 既存基本スタイルで表現できない場合は、場当たり的なCSSや独自レイアウトを追加せず、ページ固有CSSまたは共通設計変更が必要であることを先に報告する。
- 3つのMAINを混ぜて独自レイアウトを作らない。
- `shared/template` の共通層を勝手に変更しない。
- 指示外のファイルを変更しない。
- 後勝ちselector、理由のない `!important`、inline style、fix/temp/override/patch用classによる暫定修正を行わない。
- `shared/template` はデザイン・構造・共通挙動の参照元として扱い、外部パスへの実行時依存を作らない。
- ProjectButler固有の `projectbutler.css` / `projectbutler.js` はこの製品内でのみ扱い、他製品へ横展開しない。
- 製品実装の副作用として `rules` / `shared` を変更しない。

UI作業では、このファイルだけを読んで実装を開始してはいけない。必ず上記の `rules` と `shared/template` を実ファイルとして参照する。

## Completion Guard

製品実装の完了前に、変更境界を確認する。

最低限:

```text
Primary:
  git status --short
  git diff --check

rules:
  git status --short
  git diff --check

shared:
  git status --short
  git diff --check
```

製品実装タスクでは `rules` / `shared` に意図しない変更がある状態で完了報告しない。
