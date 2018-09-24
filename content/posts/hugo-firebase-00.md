---
title: "Hugo+FirebaseHosting+GitHub+CircleCI でブログ作って運用する(0)"
slug: "hugo-firebase-00"
authors: ["nacco"]
date: "2018-09-23"
description: "静的サイトジェネレータでブログをたててみたシリーズ"
# Catergory Tech Social Life
categories:
- Tech
tags:
- Hugo
- Firebase Hosting
- GitHub
- CircleCI
- Static Site Generator
- Static Hosting
draft: false
cover: /images/hugo-firebase-00.png
---

はじめまして！[Nacco](https://twitter.com/climbing_nacco)です。＼＼ ٩( 'ω' )و ／／

このブログを構築するまでの流れを最初の記事にすることにしました！Hugo(静的サイトジェネレータ)で構築しています。

序章(この記事)＋4章に分けて書きます。

---
- **0章 ブログのシステム構成について**
- 1章 Hugoで生成したHTMLをローカル環境にデプロイ、表示 (執筆中)
- 2章 Hugoで生成したHTMLをFirebase Hostingに手動デプロイ (執筆中)
- 3章 Hugoで生成したHTMLをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ (執筆中)
- 4章 テーマ選び、記事テンプレ作り (執筆中)

---
## 0章 ブログのシステム構成について

### この記事のねらい

ブログのシステム構成、各サービスがどういうものなのかを理解し、最終的には「**同種サービスの別構成も考えられる**」ようになるのがねらいです。
Hugo(**静的サイトジェネレータ**)が興味の中心なので、それを中心にまとめます。

### システム構成

こういう構成を考えました！

{{< figure src="/images/kousei.png" alt="ブログ自動デプロイのシステム構成" >}}


### システム構成 (実装)

選定したサービスを構成にあてはめました。

- 静的サイトジェネレータ　**Hugo**
- 静的ホスティング　**Firebase Hosting**
- 変更管理ツール　**GitHub**
- 実行ツール　**CircleCI**

{{< figure src="/images/kousei2.png" alt="ブログ自動デプロイのシステム構成" >}}


### 静的サイトジェネレータについて
{{< figure src="/images/static-site-generator.png" alt="静的サイトジェネレータとは？" >}}

#### 静的サイトジェネレータ is 何？

静的サイトジェネレータとは、Markdownテキスト**※1**などの「**コンテンツ**」とデザイン、レイアウト情報などを含む「**テンプレート**」を合わせてHTMLファイル、CSSファイル、javascriptファイルなどから成る「**静的サイト**」を生成するツールです。
「コンテンツ」と「テンプレート」は完全に分離した状態で**ローカルマシンで管理**します。


静的サイトジェネレータの種類により、おおまかにテンプレートとジェネレータの実装が異なります。このあと比較してみたいと思います。

{{% text s="0.8" color="#7058a3" %}}
**※1** Markdownは軽量マークアップ言語。HTML生成用に開発され、現在はいろいろな用途で使用されています。**可読性を保持したまま手早く「文書構成」を記す**のに向いています。
{{% /text %}}

ブログといえばWordpress(**CMS**)を思い浮かべます。CMSと比べて、「静的サイトジェネレータ」にはどのようなメリットがあるのでしょうか？

{{% center %}}
**静的サイトジェネレータのメリット**
{{% /center %}}


- **セキュリティ脆弱性の心配が少ない**
    - プログラムをサーバサイドに設置しないので、第三者による脆弱性の悪用のリスクが減る**※2**
- **バックアップや更新作業が不要**
    - コンテンツ(記事)はMarkdown形式テキストでローカル保存、バージョン管理システム等で管理可能
    - 静的サイトジェネレータ本体の更新が必要になる頻度も低い**※3**
- **高速に読込みができる**
    - 静的サイトは読込みが速い！
    - この記事の読み込み時間は　およそ**534ms**！
- **スケールアウトが容易**
    - 静的サイトはCDNで配信できるのでトラフィック増加時にスケールアウトできる
- **Markdown記法が利用可能なツールが多種ある**
    - コンテンツの転用が簡単！
    - アイディア出し、レビューは[HackMD](https://hackmd.io/)を利用
        - HackMDはMarkdownテキストの**共同編集が可能**なので、他者と**コンテンツのアイディア出し、レビュー**をしながら執筆を進められた
    - Markdown記法を使用できる**プレゼンテーションツール**を使ってプレゼン資料として流用もできる
        - GitHubと相性の良い[GitPitch](https://gitpitch.com/docs/getting-started/tutorial/)などのプレゼンテーションツール

個人的に、最後にあげたMarkdown記法ツールの展開がうれしいです。例えば、この記事の内容をそのまま[インフラ勉強会](https://wp.infra-workshop.tech/)での発表に使えないかなと思ってます！


{{% text s="0.8" color="#7058a3" %}}
**※2** SSR(サーバーサイドレンダリング)を導入すると別途サーバ側の堅牢性の考慮が必要になるかもしれません。これはJAMstackでSPA(シングルページアプリケーション)を作ることになったら調べます！

**※3** 実行ツール上で静的サイトジェネレータを展開する場合、ローカル環境とバージョンを揃える工夫が必要。くわしくは3章で書く予定です
{{% /text %}} 

{{% center %}}
**静的サイトジェネレータのデメリット**
{{% /center %}}


- CMS経由のコンテンツ投稿に比べて、静的サイトジェネレータによる投稿(デプロイ)には技術の習得が必要(基本的にはすべて**CUI操作**)**※4**
- 静的ホスティングを利用するので、メールフォームや検索機能などの動的機能は外部API連携、FaaSなどを利用する必要がある
- ECサイト構築や、コンテンツのマネタイズには向かない

{{% text s="0.8" color="#7058a3" %}}
**※4** [Publii](https://getpublii.com/)や[Lektor](https://www.getlektor.com/)のようなダッシュボードからのコンテンツ作成やリモートサーバへのデプロイが簡単にできるような静的サイトCMS(ローカルマシン用)も出ています
{{% /text %}} 


#### 静的サイトジェネレータの種類
{{< figure src="/images/ssg-logos.png" alt="静的サイトジェネレータの種類" >}}

先ほどあげた、ジェネレータの実装言語、テンプレートのフレームワーク、特徴などを一覧にしました。

| 種類                                                                                       | 実装言語   | Template FW                                 | 特徴                                             |
| :----------------------------------------------------------------------------------------: | ---------- | ------------------------------------------- | ------------------------------------------------ |
| **[Hugo](https://gohugo.io/)**                                                             | Go         | Go                                          | 機能はシンプル。HTML生成がとにかく高速           |
| **[Jekyll](https://jekyllrb.com/)**                                                        | Ruby       | [Liquid](https://shopify.github.io/liquid/) | 2017年一番人気。機能やテーマが充実している。     |
| **[Gatsby](https://www.gatsbyjs.org/)**                                                    | JavaScript | [React](https://reactjs.org/)               | PWA (Progressive Web App)を生成できる。          |
| **[Next.js](https://nextjs.org/)**                                                         | JavaScript | React                                       | Server Side Rendering(SSR)が初期実装されている。 |
| **[Nuxt.js](https://ja.nuxtjs.org/)**                                                      | JavaScript | [Vue](https://jp.vuejs.org/index.html)      | Vue.jsアプリケーションを静的に生成できる。       |
| **[Expose](https://github.com/Jack000/Expose)** ([demo](http://jack.ventures/2014/japan/)) | Bash       | HTML                                        | 写真、動画のギャラリーサイトを生成できる。       |
| **[GitBook](https://www.gitbook.com/)** ([demo](https://docs.gitbook.com/))                | JavaScript | [Jinja2](http://jinja.pocoo.org/docs/2.10/) | ドキュメントサイトを生成できる。                 |

今回は[Hugo](https://gohugo.io/)を選択しました。
理由は、「**とにかく爆速だ**」と噂があったことと、これからgolangのプログラミングを勉強するにあたって、[golangで実装している](https://github.com/gohugoio/hugo)静的サイトジェネレータとして興味がありました。

詳しい比較は[StaticGen](https://www.staticgen.com/)がまとまっています。Gatsby、Next.js、Nuxt.jsを理解するには**[JAMstack](https://jamstack.org/)**関連の知識が必要だと思ったので、宿題にします。シングルページアプリケーション(SPA)を作っていくなら、js系のジェネレータを触っていくのも良いかと思います。

Expose、GitBookは用途特化のジェネレータです。
HugoやJekyllはテンプレートテーマが充実していて、ブログ以外の用途にも使いやすい印象です。
 
### 静的ホスティングについて
{{< figure src="/images/shosting-logos.png" alt="静的ホスティングについて" >}}

次に静的ホスティングについてです。
それぞれ出自が違うので、得意分野が異なりますが、CLIツールは必ずついています。

| 種類                                                                                                                                                                              | 特徴                                                                                                                                                                                                                                                                             |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[Now](https://zeit.co/now)**                                                                                                                                                    | デプロイの統合CLI環境。「now」の3文字コマンドで手動デプロイ。[GitHub App](https://github.com/apps/now)をGitHubリポジトリにインストールすると、masterブランチにgit pushするごとにNowに自動デプロイできる。mergeのpull requestの承認でもデプロイ可。revertでロールバックもできる。 |
| **[Firebase Hosting](https://firebase.google.com/docs/hosting/?hl=ja)**                                                                                                           | CLIで簡単にinit/deployできる。モバイル向けBackend(MBaaS)を提供するFirebaseの一機能。                                                                                                                                                                                                                                                   |
| **[AWS S3](https://aws.amazon.com/jp/s3/)**                                                                                                                                       | WEBサイトホスティング用に設定して利用可能な[クラウドオブジェクトストレージ](https://aws.amazon.com/jp/what-is-cloud-object-storage/)。WAFを作成してCloudFrontに適用など、AWSはとにかくサービスが多彩なので連携して利用できる。                                                   |
| **[Netlify](https://www.netlify.com/)**                                                                                                                                           | 静的ホスティング専用サービス。ドラッグ＆ドロップでのデプロイやCLI操作も可能。GitHubなどのGitリポジトリと連携。HTTP/2による配信に対応                                                                                                                                             |
| **[GitHub Pages](https://pages.github.com/)**                                                                                                                                     | gh-pagesブランチを作成し、git pushすることでデプロイ。変更管理ツール、実行ツールを包括                                                                                                                                                                                           |
| **[GitLab Pages](https://about.gitlab.com/features/pages/)**                                                                                                                      | GitHub Pagesと同様のサービス。プライベートリポジトリも無料で作成できる。                                                                                                                                                                                                         |
| **[Bitbucket](https://confluence.atlassian.com/bitbucket/publishing-a-website-on-bitbucket-cloud-221449776.html#PublishingaWebsiteonBitbucketCloud-ConfigureaWebsiteforHosting)** | GitHub、GitLab同様Hostingサービスがあるところまでは確認できましたが、それ以上は今回調査不足…                                                                                                                                                                                     |

**Netlify**はビルドインされている静的サイトジェネレータが多数あり、ボタンひとつでGitHubリポジトリを作成、サイトを構築できます。**Firebase Hosting**や**AWS S3**は多機能なマネージドサービスの一機能として静的ホスティングを提供しています。

**Now**、**Netlify**、**GitHub Pages**、**GitLab Pages**はgit pushをトリガーにして自動デプロイができるので、今回のシステム構成のステップをいくつか省略できます！

今回はステップごとにツールを把握したかったので、ホスティングサービス単体で[Firebase Hosting](https://firebase.google.com/docs/hosting/?hl=ja)を選択しました。
シングルページアプリケーション(SPA)の開発に興味があってFirebaseを選びましたが、静的サイトジェネレータを利用する場合、コンテンツの移行が容易なので、料金形態や使用感で引っ越しも検討します。

### 変更管理ツールについて
{{< figure src="/images/vcs-logos.png" alt="変更管理ツールについて" >}}

**[GitHub](https://github.com/)**、**[GitLab](https://about.gitlab.com/)**、**[Bitbucket](https://bitbucket.org/)**など。

WEB上でGitリポジトリを作成、管理できます。今回「バージョン管理システム(VCS)」と呼んでいないのは、バージョン管理自体に重きを置いた構成ではなく、**コンテンツの変更(push)をトリガーにしてビルド・デプロイ実行をキックするWebhookの役割**を担っているからです。

[GitHub](https://github.com/)を選択しました。今回私は、はじめて[リポジトリ](https://github.com/nacco-bron/naccoblog)を作りました…！がんばって[草生やします](https://qiita.com/sta/items/2c1f0252a6a9ce5e2087)！！

### 実行ツールについて
{{< figure src="/images/citools-logos.png" alt="実行ツールについて" >}}

**[CircleCI](https://circleci.com/)**、**[Wercker](www.wercker.com/)**、**[TravisCI](https://travis-ci.org/)**など。

静的サイトの自動デプロイのため、Dockerなどの**コンテナを使ってビルドやデプロイを実行するツール**を選定しました。

実行ツールはGitHubなどの変更管理ツールからpushを受けると次のような処理を実行します。

1. コンテナ上で静的サイトジェネレータをビルドして、HTMLなど静的コンテンツを生成する
2. コンテナ上で静的ホスティングのCLIをインストールし、デプロイを実行

CI/CDツールと呼んでいないのは、今回ビルド・デプロイ実行ツールとして利用しているからです。今回は[CircleCI](https://circleci.com/)を選びました。実行ツールの設定は、ローカルでの実行結果との差異が発生する等トラブルが起きやすいところでした。

くわしくは、3章にまとめます。

### まとめと初投稿の反省。
{{< figure src="/images/ending-00.png" alt="まとめと初投稿の反省。" >}}

以上、静的サイトジェネレータ＋静的ホスティング＋変更管理ツール＋実行ツールの役割と種類をまとめてみました！
実際の構築手順は次回1章から説明していきます。

単純に「ブログを書きたい」「記事をシェアしたい」だけならば、はてなブログなど無料ブログサービスも選択肢として十分アリです。(というかそちらが王道ですね。)
調べていく中で「モバイル環境でもストレスがないこと」「コードで管理すること」「自動化しやすいこと」が静的サイトジェネレータで構築するWEBシステムには集結されていて、技術トレンドを知る上で、やって良かった！と思うところです。

ここからは反省点。

実は、3章までの**構築作業自体は3時間弱ほどで完了**しています。

私がAdminをやっているオンラインコミュニティ「インフラ勉強会」で構築作業のナビゲータをやってくださった方がいたので短時間で構築できましたが、
記事執筆については全然捗らず…1章分で40時間は使っています**(´・ω・`;)

もう少し軽快に記事書いていけるように「HackMDのテンプレを作った話」もまたの機会に記事にします！
最後までお付き合いいただき、ありがとうございました！

～静的サイトジェネレータでブログをたててみたシリーズ～
次回は、**1章 Hugoで生成したHTMLをローカル環境にデプロイ、表示**です！


### 参考資料
- [静的サイトジェネレーター比較「StaticGen」](https://www.staticgen.com/)
- [【git】GitHubにpushしたらブランチ毎に自動デプロイする仕組みをさくらVPS上に作ってみた](https://y-hilite.com/2812/)
- [Now でクラウドの複雑さから解放されよう、今すぐに](https://qiita.com/aggre/items/f0cb9f8b8e8c54768e50)
- [Firebase Hostingにブログを移行した](https://blog.haramishio.xyz/entry/migrate-firebase-hosting)
- 20180317-無料でサーバーレス＆自動化！？ブログを簡単にgoogle-firebase-hostingにデプロイするよー(indoctorさん:インフラ勉強会内セッション)
- [Static Site Generators | DEVOPEDIA](https://devopedia.org/static-site-generators)
- [Bitbucketのプライベートリポジトリで管理しているJekyllで作った静的サイトを、無料のCIサービスwerckerを使って自動でS3にデプロイする](http://katahirado.hatenablog.com/entry/2014/04/27/131431)
- [Now + GitHub: Push to Deploy, Master Aliasing, Instant Rollbacks](https://zeit.co/blog/every-push-now)
- [人気CIツール比較まとめ【2015年12月版】](https://qiita.com/hiro_koba_jp/items/282e3b2e534f4bc22d64)
- [How to host Hugo static website generator on AWS Lambda](http://bezdelev.com/post/hugo-aws-lambda-static-website/)
- [継続的デリバリー ツールの統合](https://cloud.google.com/container-registry/docs/continuous-delivery?hl=ja)
