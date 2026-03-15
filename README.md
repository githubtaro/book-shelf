# BookShelf - 蔵書管理アプリ

家族の蔵書を管理するWebアプリケーション。  
スマホのブラウザでISBNバーコードをスキャンして、書籍情報を自動取得・登録できます。

## 機能

- **バーコードスキャン**: スマホカメラでISBNバーコードを読み取り
- **書籍情報自動取得**: Google Books API + openBD から書籍情報・表紙画像を取得
- **蔵書一覧**: サムネイル付きリスト表示
- **検索・フィルタ**: タイトル・著者・ISBN検索、場所・カテゴリフィルタ
- **重複検出**: 同じISBNの本を検出
- **保管場所管理**: 部屋単位で本の場所を管理
- **データバックアップ**: JSON形式でエクスポート/インポート

## データ保存について

- データはブラウザの **IndexedDB** に保存されます（端末内のみ）
- ブラウザのデータを削除すると蔵書データも消えます
- **必ず定期的にJSONエクスポートでバックアップを取ってください**

## GitHub Pages へのデプロイ手順

### 1. GitHubリポジトリを作成

```bash
# 新しいリポジトリを作成（GitHub上で "bookshelf" など）
git init bookshelf
cd bookshelf
```

### 2. ファイルを配置

このフォルダの中身をリポジトリにコピー:

```
bookshelf/
├── index.html      ← メインアプリ（これ1ファイルで動く）
├── manifest.json   ← PWA用マニフェスト
└── README.md       ← このファイル
```

### 3. GitHubにプッシュ

```bash
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/bookshelf.git
git push -u origin main
```

### 4. GitHub Pages を有効化

1. GitHubのリポジトリページを開く
2. **Settings** → **Pages**
3. **Source** で **Deploy from a branch** を選択
4. **Branch** で **main** / **/ (root)** を選択
5. **Save** をクリック
6. 数分後に `https://YOUR_USERNAME.github.io/bookshelf/` でアクセス可能

### 5. スマホでアクセス

1. スマホのブラウザで上記URLを開く
2. **ホーム画面に追加** でアプリっぽく使える（PWA対応）

## バーコードスキャンについて

- **HTTPS必須**: カメラアクセスにはHTTPS接続が必要です。GitHub Pagesは自動的にHTTPSなのでOK。
- ローカルPC（`http://localhost`）でも動作します。
- `http://192.168.x.x` など HTTP のIPアクセスではカメラが使えません。

## Phase 2 への移行

将来的にLaravel + サーバ構成に移行する場合：

1. アプリの「設定」からJSONエクスポート
2. Laravel側でインポート機能を実装
3. JSONデータをLaravelのDBにインポート

フロントのUI・ロジックは参考にしつつ、Blade + Tailwindで再構築。

## 技術構成

| 要素 | 技術 |
|------|------|
| フロントエンド | HTML + Tailwind CSS (CDN) |
| バーコード読取 | html5-qrcode |
| データ保存 | IndexedDB（ブラウザ内） |
| 書籍情報API | Google Books API + openBD |
| ホスティング | GitHub Pages（無料） |
