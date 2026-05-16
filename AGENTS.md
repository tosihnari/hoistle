# このプロジェクトについて

プロトタイプを作成するプロジェクト。

---

## ルール

### UIはデジタル庁デザインシステムを使う

`design-system/design-system-example-components-html-main/` のHTMLスニペットとCSSを流用する。UIを一から独自に作らない。

### HTMLファイルは `prototype/` に置く

命名規則: `{2桁番号}_{画面名}.html`（例: `01_login.html`）

### 詳細ルール

→ [docs/prototype-rules.md](docs/prototype-rules.md)

### 共通CSS（prototype.css）の運用ルール

全ページ共通のレイアウト・コンポーネントスタイルは `prototype/prototype.css` に集約されている。
新しい画面を作る際・既存画面を編集する際は必ず参照し、**共通スタイルをページ内に重複して書かない**。

→ [docs/prototype-css.md](docs/prototype-css.md)

### ユーザー理解（CJM）

画面作成前に必ず参照する。ユーザーの行動・心理・文脈が記載されている。

→ [docs/customer-journey-map.md](docs/customer-journey-map.md)

### 画面要件メモ

各画面に入れたい要素・機能・アイデアをまとめたもの。確定仕様ではなく参考情報として使う。

→ [docs/screen-requirements.md](docs/screen-requirements.md)
