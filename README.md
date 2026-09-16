# plone.jp

Plone Users Group Japan（Plone研究会）の1ページの案内サイト。Astro + Tailwind CSSで静的HTMLを生成し、Cloudflare Pagesで配信します。ブラウザー側JavaScriptやデータベースは不要です。

## ローカル開発

Node.jsは `.node-version` を参照してください。

```sh
npm ci
npm run dev
```

開発サイト: http://localhost:4321

```sh
npm run check
npm run build
npm run preview
```

ビルド成果物は `dist/`。GitHub Actionsでpush・pull request時に型チェックとビルドを実行します。

## 内容の更新

- `src/pages/index.astro`: 本文、リンク、SEO情報
- `src/styles/global.css`: Tailwind CSSとサイトのスタイル
- `public/`: 公式ロゴ、アイコン、robots.txt、sitemap.xml、Cloudflare Pages用ヘッダー

イベントの日程や活動の最新情報は https://plonejp.connpass.com/ に追記します。このサイトはその入口として運用します。Slackの公開招待URLは置かず、connpassグループ経由で招待を依頼する案内にしています。

## GitHub → Cloudflare Pages

リポジトリ: https://github.com/plonejp/plonejp-page

1. コードをGitHubにpushします。
2. Cloudflare管理画面の **Workers & Pages** から **Pages** のプロジェクトを作成し、GitHubリポジトリ `plonejp/plonejp-page` を接続します。WorkersではなくPagesを選択してください。
3. 本番ブランチを指定して、以下のビルド設定を保存します。

| 項目 | 値 |
| --- | --- |
| Framework preset | Astro |
| Build command | `npm run build` |
| Build output directory | `dist` |
| Root directory | リポジトリルート（空欄） |
| Environment variable | `NODE_VERSION=22.22.1` |

4. 発行された `*.pages.dev` で表示・リンクを確認します。以降はGitHubへのpushで自動ビルド・配信されます。

静的サイトのため、Cloudflare用AstroアダプターやAPIキーをリポジトリに追加する必要はありません。

## plone.jp の接続

1. 既存サイトとDNS設定を確認し、切り替え前の設定を控えます。メール用MX/TXTなど、Web以外のレコードは維持します。
2. apexドメイン `plone.jp` のPages利用には、同じCloudflareアカウントでDNSゾーンを管理し、ネームサーバーをCloudflareへ向ける必要があります。すでに移管済みなら変更不要です。
3. Pagesプロジェクトの **Custom domains → Set up a domain** から `plone.jp` を追加します。ダッシュボードで登録してからDNSの案内に従い、証明書とドメインが有効になるまで確認します。既存Webサイトの切り替えになるため、公開タイミングに合わせて実施します。
4. `https://plone.jp/` で表示・HTTPS・リンクを確認します。
5. `www.plone.jp` も必要なら別途カスタムドメインとして登録し、CloudflareのRedirect Rulesで `https://plone.jp` に恒久リダイレクトします。既存サイトのURLを引き継ぐ場合は、対応先を確認して `public/_redirects` に個別の301ルールを追加してください。

canonical、OG URL、sitemapは `https://plone.jp/` を指定済みです。コード作成だけではGitHubへのpush、Cloudflare連携、DNS切り替えは行われません。

## フォント

Ploneの公式ロゴガイドラインに記載されたDINの雰囲気を参考に、英数字はBarlow、日本語はNoto Sans JPを採用。タイトルや装飾内の「Plone」、公式サイトのドメイン表記にはDIN系のD-DIN Boldを指定しています。日本語見出しは600のウェイトを基本に、プロポーショナル字形と詰めた字間・行間を使用しています。ロゴ自体は公式SVGのままです。

フォントはFontsourceのパッケージから読み込み、ビルド成果物に同梱します。閲覧時のGoogle Fontsへの通信は発生しません。両フォントのライセンスは各パッケージのLICENSEを参照してください。

D-DINはDattoがMonotypeに依頼して公開したDIN系の書体です。PloneロゴのDINと同一のフォントファイルではありません。取得元: https://github.com/amcchord/datto-d-din 。`public/fonts/d-din/` にWebフォント・COPYING.txt・FONTLOG.txtを同梱しています（SIL OFL 1.1）。

## ロゴ

公式サイトから取得したSVGを改変せず同梱しています。

- ロゴ: https://plone.org/static/media/logo.cab945d8.svg
- アイコン: https://plone.org/icon.svg
- ロゴガイドライン: https://plone.org/foundation/copyright-licensing-logo

Plone and the Plone logo are registered trademarks of the Plone Foundation. ロゴを含む第三者素材の権利は各権利者に帰属します。

## 参考ドキュメント

- [AstroのTailwind CSS設定](https://docs.astro.build/en/guides/styling/#tailwind)
- [Cloudflare PagesへのAstroデプロイ](https://developers.cloudflare.com/pages/framework-guides/deploy-an-astro-site/)
- [Cloudflare Pagesのカスタムドメイン](https://developers.cloudflare.com/pages/configuration/custom-domains/)
