# AGENTS.md

このリポジトリでAI・Codex・その他エージェントがUI・プロトタイプを作成または修正する場合は、実装開始前に必ず以下を確認する。

## Codex Project Preflight

標準構成:

```text
Codex Project
├─ ProjectButler-Prototype  ← Primary
├─ rules
└─ shared
```

UI・プロトタイプ作業前に、source folder `rules` と `shared` が見えていることを確認する。

どちらかが見えない場合は実装を開始せず、source folder不足を報告する。`rules` / `shared` をこのrepoへコピー・submodule化・推測代替してはいけない。

製品実装中は `rules` と `shared` を参照専用として扱う。共通ルール・テンプレート・アイコン自体の変更を明示された場合だけ変更する。

## Required Read Order

1. このリポジトリの `PROJECT.md` / `README.md` / 関連設計資料
2. `rules/AGENTS.md`
3. `rules/cluewly-github-rules.md`
4. `rules/codex-project-setup.md`
5. `shared/AGENTS.md`
6. `shared/template/AGENTS.md`
7. `shared/template/PROJECT.md`
8. `shared/template/README.md`
9. `shared/template/docs/AI_PAGE_CREATION_CHECKLIST.md`
10. `shared/template/docs/AI_SAFE_EDITING_GUIDE.md`
11. 対象HTML / CSS / JS

## UI / Prototype Rule

- 新規ページは `SIDE NAV` / `SIDE CONTENT` / `MAIN ONLY` の3つから適切なものを先に選ぶ。
- 選択は見た目ではなく左側領域の役割で判断する。
- 既存フォント・カラー・余白・角丸・シャドウ・ボタン・フォーム・パネルなど、`shared/template` の既存基本スタイルを優先する。
- 既存基本スタイルで表現できない場合は、場当たり的なCSSや独自レイアウトを追加せず、ページ固有CSSまたは共通設計変更が必要であることを先に報告する。
- 3つのMAINを混ぜて独自レイアウトを作らない。
- `shared/template` の共通層を勝手に変更しない。
- 指示外のファイルを変更しない。
- 後勝ちselector、理由のない `!important`、inline style、fix/temp/override/patch用classによる暫定修正を行わない。

UI作業では、このファイルだけを読んで実装を開始してはいけない。必ず上記の `rules` と `shared/template` を実ファイルとして参照する。
