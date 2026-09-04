# AGENTS.md

このリポジトリでAI・Codex・その他エージェントがUI・プロトタイプを作成または修正する場合は、実装開始前に必ず以下を確認する。

## Required Read Order

1. このリポジトリの `PROJECT.md` / `README.md` / 関連設計資料
2. Cluewly共通ルール `ZUNZUNZURAN/rules`
3. `ZUNZUNZURAN/shared` の `AGENTS.md`
4. `ZUNZUNZURAN/shared/template/AGENTS.md`
5. `ZUNZUNZURAN/shared/template/PROJECT.md`
6. `ZUNZUNZURAN/shared/template/README.md`
7. `ZUNZUNZURAN/shared/template/docs/AI_PAGE_CREATION_CHECKLIST.md`
8. `ZUNZUNZURAN/shared/template/docs/AI_SAFE_EDITING_GUIDE.md`
9. 対象HTML / CSS / JS

## UI / Prototype Rule

- 新規ページは `SIDE NAV` / `SIDE CONTENT` / `MAIN ONLY` の3つから適切なものを先に選ぶ。
- 既存フォント・カラー・余白・角丸・シャドウ・ボタン・フォーム・パネルなど、`shared/template` の既存基本スタイルを優先する。
- 既存基本スタイルで表現できない場合は、場当たり的なCSSや独自レイアウトを追加せず、ページ固有CSSまたは共通設計変更が必要であることを先に報告する。
- 3つのMAINを混ぜて独自レイアウトを作らない。
- `shared/template` の共通層を勝手に変更しない。
- 指示外のファイルを変更しない。
- 後勝ちselector、理由のない `!important`、inline style、fix/temp/override/patch用classによる暫定修正を行わない。

UI作業では、このファイルだけを読んで実装を開始してはいけない。必ず上記の共通ルールと `shared/template` を先に参照する。
