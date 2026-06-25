# AKIBA JAPAN — プロジェクト学習メモ

> このファイルはプロジェクト構造の学習メモです。いつでも参照できます。

---

## プロジェクト全体の構造

```
akiba japan/
├── package.json          ← プロジェクトの設定・使用ライブラリ一覧
├── astro.config.mjs      ← Astroの設定ファイル
├── public/               ← 画像・faviconなど静的ファイル（そのままコピーされる）
├── dist/                 ← ビルド結果（自動生成・手動編集禁止）
└── src/
    ├── pages/            ← 各ページファイル（1ファイル = 1ページ）
    ├── layouts/          ← 全ページ共通のテンプレート
    ├── components/       ← ヘッダー・フッターなど再利用パーツ
    ├── data/             ← データ（店舗リストなど）
    └── styles/           ← グローバルCSS
```

---

## 1. `package.json` — プロジェクトの基本情報

### よく使うコマンド

| コマンド | 意味 |
|---|---|
| `npm run dev` | ローカルで開発サーバー起動（ http://localhost:4321 ） |
| `npm run build` | `dist/` フォルダにHTMLをビルド |
| `npm run preview` | ビルド結果をプレビュー |

### 使用ライブラリ

- `astro` — フレームワーク本体
- `tailwindcss` — CSSユーティリティ（classで直接スタイル指定）
- `@tailwindcss/vite` — AstroとTailwindの連携プラグイン

---

## 2. `src/layouts/BaseLayout.astro` — 全ページ共通テンプレート

### ファイルの構造

```
---
  (JavaScriptコード — ビルド時に実行)
---

  (HTMLコード — ブラウザに送られる)
```

### 受け取れるデータ（Props）

| プロパティ | 必須 | 説明 |
|---|---|---|
| `title` | ✅ | ブラウザタブのタイトル |
| `description` | — | Google検索の説明文（未指定時はデフォルトが使われる） |
| `ogTitle` | — | SNSシェア時のタイトル |
| `ogDescription` | — | SNSシェア時の説明文 |
| `ogImage` | — | SNSシェア時の画像 |
| `noIndex` | — | `true` にするとGoogleにインデックスされない |

### HTMLの骨格

```html
<body>
  <Header />    ← ナビゲーションバー
  <main>
    <slot />    ← 各ページのコンテンツがここに入る
  </main>
  <Footer />    ← フッター
</body>
```

`<slot />` = 「穴」。`index.astro` や `about.astro` などの内容がここに差し込まれる。

### 各ページでの使い方

```astro
---
import BaseLayout from "../layouts/BaseLayout.astro";
---
<BaseLayout title="ページタイトル" description="説明文">
  <!-- ここにページの内容 -->
</BaseLayout>
```

---

## 3. `src/components/Header.astro` — ナビゲーションバー

### ナビリンクの追加・変更

ファイル上部の `navLinks` 配列を編集するだけでOK。

```js
const navLinks = [
  { href: "/",       label: "ホーム" },
  { href: "/about",  label: "組織について" },
  { href: "/shops",  label: "加盟店" },
  // ↓ 新しいページを追加する場合はここに追記
  { href: "/news",   label: "ニュース" },
];
```

**重要：** `href` は必ず一意（ユニーク）にすること。同じ `href` が2つあると、両方のリンクがアクティブ状態（赤色）になってしまう。

### 主な変更箇所

| 変更したい内容 | 場所 |
|---|---|
| メニューの追加・削除・順番 | `navLinks` 配列 |
| ロゴテキスト "AKIBA JAPAN" | `<span>` タグ内 |
| ナビ背景色 | `bg-black` クラス |
| ナビの高さ | `h-16` クラス |
| ナビを上部固定にする/しない | `sticky top-0` クラス |

### 仕組み

- **デスクトップ** → `hidden lg:flex`（大画面のみ表示）
- **モバイル** → ハンバーガーボタン（☰）をクリックでメニュー展開
- **アクティブ状態** → 現在のURLと `href` が一致したリンクだけ赤色になる

---

## 4. `src/components/Footer.astro` — フッター

### リンクの構成（3列）

```js
const nav1 = [ ... ]  // 「組織」列
const nav2 = [ ... ]  // 「活動」列
const nav3 = [ ... ]  // 「サポート」列
```

### SNSリンクの変更

```js
const sns = [
  { href: "#", label: "X (Twitter)", icon: `<svg .../>` },
  { href: "#", label: "Instagram",   icon: `<svg .../>` },
  { href: "#", label: "YouTube",     icon: `<svg .../>` },
];
```

`href: "#"` を実際のURLに変更するだけでリンクが有効になる。

### 主な変更箇所

| 変更したい内容 | 場所 |
|---|---|
| 各列のリンク | `nav1` / `nav2` / `nav3` 配列 |
| 列のタイトル（"組織"など） | `<h3>` タグ |
| ロゴ下のキャッチコピー | `<p>` タグ内のテキスト |
| SNSのリンク先 | `sns` 配列の `href` |
| コピーライトの名前 | 最下部の `<p>` タグ |

### 注意点

年号 `{year}` は `new Date().getFullYear()` で自動更新される。手動で変更不要。

---

## 新しいページを追加する手順

```
1. src/pages/ページ名.astro を作成
2. npm run dev でブラウザ確認
3. Header.astro の navLinks に追記
4. Footer.astro の nav1/nav2/nav3 のどれかに追記
```

### 新しいページの最低限のテンプレート

```astro
---
import BaseLayout from "../layouts/BaseLayout.astro";
---
<BaseLayout title="ページタイトル | AKIBA JAPAN">
  <section class="max-w-7xl mx-auto px-4 py-20">
    <h1 class="text-4xl font-bold text-white">見出し</h1>
    <p class="text-white/70 mt-4">本文テキスト</p>
  </section>
</BaseLayout>
```

---

## Tailwind CSS — よく使うクラス一覧

### 色

| クラス | 意味 |
|---|---|
| `text-white` | テキストを白に |
| `text-white/70` | テキストを白・70%透明に |
| `bg-black` | 背景を黒に |
| `text-primary` | テキストをサイトのメインカラー（赤 #e8546a）に |

### レイアウト

| クラス | 意味 |
|---|---|
| `flex` | Flexboxレイアウト |
| `grid grid-cols-4` | 4列グリッド |
| `hidden lg:flex` | スマホで非表示・大画面でflex表示 |
| `max-w-7xl mx-auto` | 最大幅を設定して中央寄せ |
| `px-4 py-8` | 左右padding 4、上下padding 8 |

### テキスト

| クラス | 意味 |
|---|---|
| `text-sm` | 小さいテキスト |
| `text-4xl` | 大きい見出し |
| `font-bold` | 太字 |
| `tracking-widest` | 文字間隔を広く |

### インタラクション

| クラス | 意味 |
|---|---|
| `hover:text-white` | ホバー時にテキストを白に |
| `transition-colors` | 色変化をなめらかに |
| `cursor-pointer` | マウスカーソルをポインターに |

---

## 次に学ぶこと

- [ ] `src/pages/index.astro` — トップページの構造
- [ ] `src/data/shops.js` — データファイルの使い方
- [ ] `src/components/ShopCard.astro` — カードコンポーネント
- [ ] `src/styles/global.css` — グローバルスタイル
- [ ] 新しいページを一から作成する実践
