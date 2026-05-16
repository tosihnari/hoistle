# プロトタイプ作成ルール

## デザイン方針

UIは一から独自に作らず、**デジタル庁が公開しているデザインシステム**を活用する。

- リポジトリ: `design-system/design-system-example-components-html-main/`
- HTMLスニペットとCSSをそのまま流用する
- 独自スタイルは最小限にとどめる（レイアウト調整のみ）

### 配色ルール：モノトーン（solid gray）を使う

プロトタイプのUIは**必ずモノトーン**で作成する。色はデザインシステムの `--color-neutral-solid-gray-*` トークンのみを使用する。

```css
/* 使ってよい色トークンの例 */
--color-neutral-solid-gray-50   /* 背景・ホバー */
--color-neutral-solid-gray-100  /* 区切り線・背景 */
--color-neutral-solid-gray-200  /* ボーダー */
--color-neutral-solid-gray-400  /* プレースホルダー・補助テキスト */
--color-neutral-solid-gray-600  /* サブテキスト */
--color-neutral-solid-gray-700  /* 本文 */
--color-neutral-solid-gray-800  /* 主要テキスト */
--color-neutral-solid-gray-900  /* 強調テキスト */
--color-neutral-white            /* 白背景 */
--color-neutral-black            /* 黒 */
```

**禁止事項：**
- `#0051D5`・`#F0F3FF` などのブランドカラー・アクセントカラーを直接指定しない
- `--color-primitive-blue-*` など blue系トークンをそのまま使わない
- blue系コンポーネント（アコーディオンアイコン等）を使う場合は CSS変数オーバーライドでグレーに変換する

```css
/* blue→gray 変換の例 */
--color-primitive-blue-1000: var(--color-neutral-solid-gray-800);
--color-primitive-blue-900: var(--color-neutral-solid-gray-700);
```

### CSSの読み込みパス（prototype/配下から参照する場合）

```html
<link rel="stylesheet" href="../design-system/design-system-example-components-html-main/src/global.css">
<link rel="stylesheet" href="../design-system/design-system-example-components-html-main/src/components/{コンポーネント名}/{コンポーネント名}.css">
```

### ⚠️ コンポーネント追加時は必ずCSSリンクを確認する

新しいデザインシステムコンポーネント（例: accordion）をHTMLに追加するとき、そのコンポーネント用CSSが `<head>` に読み込まれているか必ず確認する。リンクがないとスタイルが一切適用されない。

```html
<!-- 例: アコーディオンを使うページには accordion.css が必要 -->
<link rel="stylesheet" href="../design-system/.../accordion/accordion.css">
```

---

## フォルダー構成

```
00-4_hoistle/
├── CLAUDE.md               # Claude Code向け指示（目次）
├── docs/                   # 詳細ルール
│   └── prototype-rules.md
├── prototype/              # HTMLプロトタイプ（成果物）
│   ├── 01_login.html
│   └── 02_dashboard.html
└── design-system/          # デジタル庁デザインシステム（変更しない）
    └── design-system-example-components-html-main/
```

---

## 命名規則

### ファイル名

```
{2桁番号}_{画面名}.html
```

| ファイル名 | 意味 |
|---|---|
| `01_login.html` | 1番目の画面：ログイン |
| `02_dashboard.html` | 2番目の画面：ダッシュボード |
| `03_user-list.html` | 3番目の画面：ユーザー一覧 |

- 番号は画面の遷移順に振る
- 画面名は英語・ハイフン区切り・小文字
- **バリアント（状態違い）のファイルも必ず独立した番号を振る。元ファイルと同じ番号を使わない**
  - NG: `14_participant-event.html` のコピーを `14_participant-event-enjoy.html` にする
  - OK: 次の空き番号を使い `26_participant-event-enjoy.html` にする

### 画面タイトル（`<title>`タグ）

日本語で記載する。例: `<title>ログイン</title>`

---

## 利用可能なコンポーネント一覧

デザインシステムに含まれる主なコンポーネント:

`accordion` / `button` / `card` / `checkbox` / `date-picker` /
`form-control-label` / `heading` / `input-text` / `link` / `list` /
`notification-banner` / `radio` / `search-box` / `select` /
`table` / `textarea` など（全35種類）

コンポーネントのHTML例は以下を参照:
`design-system/design-system-example-components-html-main/src/components/{コンポーネント名}/playground.html`

---

## 要素削除時のチェックリスト

HTML要素を削除するとき、以下を必ず確認してトルツメする。

### 1. 削除した要素専用のCSSクラスも消す

`<style>` ブロックまたは `prototype.css` に、削除した要素だけが使っているCSSクラスが残っていないか確認して削除する。

### 2. 削除した要素の高さを前提にした補正値を修正する

固定配置（`position: fixed`）の要素が絡む場合、以下がセットで変わることが多い。

| プロパティ | 用途 | 対応 |
|---|---|---|
| `body { padding-bottom: Xpx }` | 固定フッターの高さ分だけコンテンツをずらす | 削除した要素の高さ分を引く |
| `.fixed-element { bottom: Xpx }` | 別の固定要素の上に重ねるオフセット | 削除した要素がなければ `0` に戻す |

**例:** ローカルナビ（66px）を削除した場合
- `body { padding-bottom: 160px }` → `94px`（ローカルナビ66px分を引く）
- `dm-input-bar { bottom: 66px }` → `bottom: 0`（ローカルナビの上に乗せる必要がなくなる）

### 3. body の背景色が意図せず露出していないか確認する

`prototype.css` の `body` はグレー（`--color-neutral-solid-gray-50`）。固定要素を削除してコンテンツ量が減ると、ページ下部にグレーが見えることがある。その場合はページのインラインスタイルで `body { background: var(--color-neutral-white); }` を指定する。
