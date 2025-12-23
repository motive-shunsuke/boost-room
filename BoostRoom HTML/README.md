# BOOST ROOM — 公開手順とStudio移行ガイド

このリポジトリには、現在の単一ファイルのサイト `index.html` が含まれています。以下は「Studio で実装する場合」と「今すぐ公開する（静的ホスティング）」の両方の手順です。

---

**選択肢 A — Studio に再実装（推奨：運用・編集が簡単）**
- 概要: Studio のデザインエディタでページを再構築し、CMS・フォーム・SEO を Studio 上で管理できます。将来的にコンテンツ差し替えや多ページ運用を考えている場合はこちらがおすすめです。

手順（要 Studio アカウント）:
1. https://studio.design/ja からサインアップ（または既存アカウントでログイン）。
2. 新しいサイトを作成 → テンプレート選択（空のプロジェクトでも OK）。
3. `index.html` を参照しながら、Editor で下記を作成:
   - ヘッダー、ヒーロー、セクション（CONCEPT、HOW TO USE、PRICE、ACCESS、フッター）をそれぞれブロック（コンポーネント）として作る。
   - 画像は Studio にアップロード（Studio.Stock かローカル画像を使用）。
   - テキストは Editor のテキストツールで追加。コピーは `index.html` からコピペ可能。
   - アニメーションやレスポンシブは Editor のプロパティで設定。Editor AI や Auto Responsive が利用可能です。
4. 外部埋め込み（Google Maps iframe）は、Studio の埋め込み/HTMLブロックを利用して追加。
5. フォームやCTAを Studio のフォーム機能で作成（送信先メールやスプレッドシート連携を設定）。
6. SEO 設定（ページタイトル、メタ説明、OG 画像）を各ページで設定。
7. プレビュー確認 → 公開（Studio のホスティングでそのまま公開可能）。独自ドメインも設定可能。

注意点:
- Tailwind のクラスや外部 CSS をそのまま Studio に読み込ませる仕組みはありません。デザインは Editor のスタイルで再現する必要があります。
- 現行の `index.html` にある細かい CSS（neon-effects、カスタム keyframes）は、Studio のデザインプロパティで近似して再現します。

メリット:
- CMS／フォーム／SEO を Studio 上で管理可能
- 非エンジニアでも更新しやすい
- 1クリック公開・独自ドメイン対応

---

**選択肢 B — 現状の `index.html` を即時公開（素早く公開したい場合）**
- 概要: 現状のファイルをそのまま静的ホスティングにデプロイします。Tailwind（CDN）や外部画像を使っているため、そのままホストすれば動作します。

推奨ホスト (簡単順): GitHub Pages, Vercel, Netlify

手順（GitHub Pages - 簡単）:
1. Git を初期化して GitHub にリポジトリを作成（例: `boost-room`）。

```bash
cd "Gemeni Website"
git init
git add .
git commit -m "Initial commit: boost room site"
# GitHub リポジトリを作成して origin を設定
git remote add origin git@github.com:YOUR_USERNAME/REPO_NAME.git
git push -u origin main
```
2. リポジトリの `Settings` → `Pages` で `main` ブランチのルートを公開設定。数分で公開されます。

手順（Vercel - 推奨、自動デプロイ）:
1. https://vercel.com/ にサインアップ
2. 新しいプロジェクトで GitHub リポジトリを選択してデプロイ（デフォルト設定で静的サイトとして動作します）

手順（Netlify）:
1. https://app.netlify.com/ にサインアップ
2. 新しいサイトを Git リポジトリからデプロイ

注意点:
- 外部画像（Unsplash）や Google Maps iframe はそのまま動作しますが、永続的に安定させるなら画像をローカルにダウンロードして `/assets` に置くことを推奨します。
- 独自ドメインを使う場合、各ホスティングのドメイン設定で DNS を追加してください。

---

**提案（私が代行でできること）**
- A1: Studio 用の再実装ガイドと、再現に必要なアセット（画像）をワークスペースにまとめる。
- A2: `index.html` をそのまま公開するための `README.md` とデプロイ手順（上記）を作成済み。
- A3: 希望ならローカルで Git リポジトリを初期化してコミット、または `netlify deploy` / `vercel` CLI 用のスクリプトを提示します（ただし公開にはあなたのアカウント認証が必要です）。

---

次のアクションを教えてください:
- 「Studio で再構築して公開」 を進めて欲しい（私がアセット準備と詳細手順を作ります）
- 「現状の `index.html` を今すぐ公開」 を進めて欲しい（私がローカルで repo を整えて公開手順のコマンドをまとめます）
- まずはスタイルや画像を最適化して欲しい（画像のローカル保存／軽量化など）

選択をいただければ、次のステップをこちらで実行します。

---

Cloudflare (Wrangler) によるデプロイ
-------------------------------------

このリポジトリには `wrangler.jsonc` が追加されています。静的サイト（`index.html`）を Wrangler v4 でデプロイするには、ローカルまたは CI で次のコマンドを実行してください:

```
npx wrangler deploy --assets=./
```

または、Cloudflare のビルド設定でデプロイコマンドを `npx wrangler deploy --assets=./` にすると、ビルド時にサイト全体がアップロードされます。

（注）ルートをアセットディレクトリに指定しているため、小さいプロジェクトであればそのまま動作します。大きなプロジェクトやビルド成果物がある場合は `./dist` のようにビルド出力フォルダを指定することを推奨します。