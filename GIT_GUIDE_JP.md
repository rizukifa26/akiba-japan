# Git & VS Code セットアップガイド
**AKIBA JAPAN プロジェクト用**

---

## 目次
1. [Git のインストール](#1-git-のインストール)
2. [Git の初期設定](#2-git-の初期設定)
3. [GitHub アカウントを VS Code に接続する](#3-github-アカウントを-vs-code-に接続する)
4. [プロジェクトをクローンする](#4-プロジェクトをクローンする)
5. [Node.js のインストールと依存関係のセットアップ](#5-nodejs-のインストールと依存関係のセットアップ)
6. [変更を保存して Push する](#6-変更を保存して-push-する)
7. [よくあるエラーと対処法](#7-よくあるエラーと対処法)

---

## 1. Git のインストール

### Windows の場合
1. [https://git-scm.com](https://git-scm.com) を開く
2. 「Download for Windows」をクリック
3. ダウンロードした `.exe` ファイルを実行
4. すべてデフォルト設定のまま「Next」→「Install」

### Mac の場合
ターミナルを開いて以下を入力：
```
git --version
```
インストール済みであればバージョンが表示されます。未インストールの場合は自動でインストールが促されます。

### インストール確認
ターミナル（またはコマンドプロンプト）で確認：
```
git --version
```
`git version 2.x.x` のように表示されれば成功です。

---

## 2. Git の初期設定

VS Code のターミナルを開きます（メニュー → **Terminal** → **New Terminal**）。

以下のコマンドを1行ずつ入力してください（名前とメールは自分のものに変更）：

```bash
git config --global user.name "あなたの名前"
```
```bash
git config --global user.email "your-email@example.com"
```

> メールアドレスは GitHub に登録したものと同じにしてください。

---

## 3. GitHub アカウントを VS Code に接続する

### 方法①：VS Code のサイドバーから接続
1. VS Code 左側の人型アイコン（**アカウント**）をクリック
2. 「GitHub でサインイン」を選択
3. ブラウザが開くので、GitHub のログイン画面でサインイン
4. 「Authorize Visual-Studio-Code」をクリック
5. VS Code に戻ると接続完了

### 方法②：GitHub Pull Requests 拡張機能を使う
1. VS Code の拡張機能アイコン（左サイドバー）をクリック
2. 検索欄に `GitHub Pull Requests` と入力
3. インストール後、画面の案内に従ってサインイン

---

## 4. プロジェクトをクローンする

### VS Code のコマンドパレットから
1. `Ctrl+Shift+P`（Mac は `Cmd+Shift+P`）を押す
2. `Git: Clone` と入力して選択
3. 以下の URL を貼り付けて Enter：
   ```
   https://github.com/rizukifa26/akiba-japan.git
   ```
4. 保存先フォルダを選択
5. 「Open」をクリックしてプロジェクトを開く

---

## 5. Node.js のインストールと依存関係のセットアップ

### Node.js のインストール
1. [https://nodejs.org](https://nodejs.org) を開く
2. 「LTS」バージョンをダウンロードしてインストール

### プロジェクトの依存関係をインストール
VS Code のターミナルで：
```bash
npm install
```

### 開発サーバーを起動して確認
```bash
npm run dev
```
ブラウザで `http://localhost:4321` を開いてサイトが表示されれば成功です。

---

## 6. 変更を保存して Push する

コードを編集した後、以下の手順で GitHub に保存します。

### VS Code の GUI で行う場合（簡単）

1. **ファイルを編集・保存**（`Ctrl+S` / `Cmd+S`）
2. 左サイドバーの **ソース管理アイコン**（木が分岐しているアイコン）をクリック
3. 変更されたファイルの一覧が表示される
4. `+` ボタンをクリックして変更をステージング（全ファイルは「Changes」横の `+`）
5. 上部のテキスト欄に変更内容を簡単に入力（例：`ショップ情報を更新`）
6. **チェックマーク（✓ Commit）** をクリック
7. 画面下の **「Sync Changes」または「Push」** ボタンをクリック

### ターミナルで行う場合

```bash
# 変更ファイルを確認
git status

# 全ファイルをステージング
git add .

# 変更内容を記録（メッセージは日本語でもOK）
git commit -m "ショップ情報を更新"

# GitHub に送信
git push
```

---

## 7. よくあるエラーと対処法

### `Permission denied` または `Authentication failed`
→ GitHub の接続が切れています。[手順 3](#3-github-アカウントを-vs-code-に接続する) を再度行ってください。

### `Please tell me who you are`
→ Git の名前・メールが未設定です。[手順 2](#2-git-の初期設定) を行ってください。

### `npm: command not found`
→ Node.js が未インストールです。[手順 5](#5-nodejs-のインストールと依存関係のセットアップ) を行ってください。

### `Updates were rejected` または `push が拒否された`
→ リモートに新しい変更があります。以下を実行してから Push：
```bash
git pull
git push
```

### `merge conflict`（競合）が発生した場合
VS Code が競合箇所をハイライトで表示します。
- 「Accept Current Change」または「Accept Incoming Change」を選択
- 保存してから再度 Commit → Push

---

## 補足：主なコマンド一覧

| コマンド | 内容 |
|---|---|
| `git status` | 変更ファイルの確認 |
| `git add .` | 全ファイルをステージング |
| `git commit -m "メッセージ"` | 変更を記録 |
| `git push` | GitHub に送信 |
| `git pull` | GitHub から最新を取得 |
| `npm run dev` | 開発サーバー起動 |
| `npm run build` | 本番用ビルド |

---

ご不明な点は担当者までお問い合わせください。

---

## Push と Pull の使い方

### Push とは？
自分のパソコンで編集したコードを **GitHub（みんなが見られる場所）に送る** ことです。

```
自分のパソコン  →→→  GitHub
```

**使うタイミング：** コードを編集・保存して、チームに共有したいとき

**手順：**
```bash
git add .
git commit -m "変更の内容を書く"
git push
```

---

### Pull とは？
GitHub にある **最新のコードを自分のパソコンに取り込む** ことです。

```
GitHub  →→→  自分のパソコン
```

**使うタイミング：** 誰かが Push した後、自分のパソコンを最新にしたいとき

**手順：**
```bash
git pull
```

---

### チームで作業する流れ

```
Aさんが編集
     ↓
git push  →  GitHub に送る
     ↓
Bさんが git pull  →  最新を受け取る
     ↓
Bさんが編集
     ↓
git push  →  GitHub に送る
     ↓
Aさんが git pull  →  最新を受け取る
```

> **大切なルール：** 作業を始める前に必ず `git pull` を実行してください。
> そうしないと、古いコードを編集してしまい、後で conflict（ぶつかり）が起きることがあります。
