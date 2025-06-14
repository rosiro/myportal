以下は **コピペ取り込み対応を追記した最新版 README.md** です。
そのまま置き換えて GitHub にコミットしてください。

```markdown
# スタートダッシュボード

静的 **HTML + JavaScript** だけで動作するパーソナルリンクポータルです。  
**このまま GitHub Pages にアップ可能です。**  
ブックマークを記載した **`sites.json`** を  
- 外部ストレージから **URL で読み込み**  
- あるいは **JSON をコピペして取り込み**  
し、カテゴリ（キー）ごとにカード表示します。  
一度取り込んだ JSON はブラウザの **`localStorage`** にキャッシュされるため、オフラインでも高速に閲覧できます。

> ⚠ **プライバシー重視の運用を推奨**  
> - `sites.json` には社内リンクや個人専用 URL が含まれる可能性があります。  
> - **このリポジトリや GitHub Pages に `sites.json` を置かないでください**。  
> - 必ず **自分が管理する非公開ストレージ**（Amazon S3、Cloudflare R2、社内 CDN など）に配置し、CORS を適切に設定してください。

---

## ✨ 主な特徴

| 機能 | 説明 |
| ---- | ---- |
| **完全クライアントサイド** | HTML 1 枚で完結。サーバー／DB／ビルド不要 |
| **外部 JSON 読み込み** | URL を入力してプライベートな `sites.json` を取得 |
| **コピペ取り込み** | JSON をその場で貼り付け → 即保存・即反映 |
| **ローカルキャッシュ** | `localStorage` に保存し、オフラインでも閲覧可 |
| **動的カテゴリ生成** | JSON キーをセクション見出しとして自動生成 |
| **レスポンシブ UI** | PC・タブレット・スマホで最適表示 |
| **MIT ライセンス** | 商用利用・改変・再配布自由 |

---

## 🗂 ファイル構成

| パス | 役割 |
| ---- | ---- |
| `index.html` | ポータル本体（HTML / CSS / JS オールインワン） |
| `README.md`  | このドキュメント（JSON は含めない） |

> **`sites.json` はコミットしません**  
> 情報漏えい防止のため、必ず外部ストレージに置くかコピペ機能で取り込んでください。

---

## 🚀 クイックスタート

### 1. ポータルを公開

1. 本リポジトリをフォーク or クローン  
2. `index.html` と `README.md` だけを残す  
3. GitHub Pages（`main` ブランチまたは `docs/` ディレクトリ）を有効化  
   - Netlify / Vercel など他の静的ホスティングでも同様にアップロードするだけです

### 2-A. 外部 URL で取り込む場合

1. **安全な場所**（S3、R2、社内 CDN…）に `sites.json` を置く（HTTPS 必須）  
2. そのサーバーで **CORS ヘッダー**  
```

Access-Control-Allow-Origin: https\://<あなた>.github.io

````
を返すよう設定  
3. ページ下部の **「sites.json の URL」** 入力欄に URL を入力 → **📥 読み込み**  
4. 以後はキャッシュから即表示。更新したいときだけ再読込

### 2-B. JSON をコピペで取り込む場合（ローカルサーバー不要）

1. `sites.json` をテキストエディタで開き、**全選択 → コピー**  
2. ダッシュボード最下部の **貼り付け用テキストエリア** にそのままペースト  
3. **✅ 貼り付けて取り込む** ボタンを押す → 即反映 & キャッシュ保存  
4. 次回以降はオフラインでもページを開くだけでリンクが表示されます  
- 新しい JSON を貼り付ければ自動で上書き保存  
- 誤取り込み時はブラウザ DevTools → Application → localStorage → `sitesCache` を削除

---

## 📝 `sites.json` フォーマット

```jsonc
{
"Project": [
 { "title": "GitHub",  "url": "https://github.com",          "description": "コードホスティング" },
 { "title": "Gitea",   "url": "https://gitea.example.com" }
],
"Daily": [
 { "title": "メール",  "url": "https://mail.example.com" }
],
"社内 Wiki": [
 { "title": "Notion",  "url": "https://notion.example.com" }
]
}
````

* **トップレベルキー** がセクション名（日本語可）
* 各要素は `title` (必須)・`url` (必須)・`description` (任意)
* JSON なのでコメントは書けません

---

## 💾 キャッシュ仕様

| 項目      | 内容                                                 |
| ------- | -------------------------------------------------- |
| 保管先     | `localStorage`（キー: `sitesCache`）                   |
| 保存タイミング | URL 読み込み成功時 / コピペ取り込み時                             |
| 読み込み順序  | ① キャッシュがあれば即描画<br>② 無ければ指定 URL から取得                |
| 更新方法    | コピペ or URL 再入力<br>もしくは DevTools で `sitesCache` を削除 |

---

## 🔧 カスタマイズ

| 対象            | 方法                                        |
| ------------- | ----------------------------------------- |
| デフォルト読み込み URL | `index.html` 内 `const SITES_JSON_URL` を編集 |
| 背景・フォント       | `<style>` セクションの CSS を編集                  |
| クイックアクション     | `<div class="quick-actions">` のリンクを変更     |
| 検索エンジン        | `performSearch()` の検索 URL を変更             |

---

## 🌐 CORS 設定例 — `Access-Control-Allow-Origin` を付ける

| ホスティング            | 設定（例）                                                                                                                                                                                                        |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Amazon S3**     | バケット → **Permissions → CORS configuration**<br>`xml<br><CORSConfiguration><CORSRule><AllowedOrigin>https://<あなた>.github.io</AllowedOrigin><AllowedMethod>GET</AllowedMethod></CORSRule></CORSConfiguration>` |
| **Cloudflare R2** | バケット → **Settings → CORS**<br>Allowed origins に `https://<あなた>.github.io`                                                                                                                                    |
| **Nginx**         | `add_header Access-Control-Allow-Origin "https://<あなた>.github.io";`                                                                                                                                          |
| **Netlify**       | ルートに `_headers`<br>`text<br>/sites.json<br>  Access-Control-Allow-Origin: https://<あなた>.github.io`                                                                                                           |

> **ワイルドカード `*` は推奨しません** — 機微情報が含まれる場合は必ずドメインを限定してください。

---

## 🔒 FAQ（セキュリティ関連）

| 質問                           | 回答                                  |
| ---------------------------- | ----------------------------------- |
| `sites.json` にパスワードを入れていい？   | **絶対に入れないでください**。キャッシュは平文です。        |
| GitHub Pages で Basic 認証できない？ | できません。認証が必要な場合は外部ストレージ側で制御します。      |
| CORS を `*` にすると？             | どのサイトからでも JSON が取得でき、漏えい時のリスクが増します。 |

---

## 📜 ライセンス

[MIT](LICENSE) – 改変・商用利用自由です。Issue / PR 歓迎！

```
```
