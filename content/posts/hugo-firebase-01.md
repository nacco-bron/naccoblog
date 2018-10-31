---
title: "Hugo+Firebase+GitHub+CircleCIでブログ作る-0_1"
slug: "hugo-firebase-01"
authors: ["nacco"]
date: "2018-09-20"
description: "静的サイトジェネレータについて"
# Catergory Tech Social Life
categories:
- Tech
tags:
- Hugo
- Firebase
- GitHub
- CircleCI
draft: false
cover: /images/hugo-firebase-00/hugo-firebase-00.png
---

こんにちは！[Nacco](https://twitter.com/climbing_nacco)です ( ･`ω･´)

[前回](../hugo-firebase-00#システム構成)のシステム構成で使った各ツールを順番に役割、種類などを紹介していきます！

今回は「**静的サイトジェネレータについて**」調べた内容です。

---

- **0章 ブログのシステム構成について**
    - [0章のねらい、システム構成について](../hugo-firebase-00)
    - **静的サイトジェネレータについて**
    - [静的ホスティングについて](../hugo-firebase-02)
    - [変更管理ツールについて](../hugo-firebase-03)
    - [実行ツールについて、0章まとめ](../hugo-firebase-04)
- [1章 Hugoで生成した静的サイトをローカル環境でプレビュー](../hugo-firebase-10)
- 2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ
- 3章 Hugoで生成した静的サイトをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ
- 4章 テーマ選び、記事テンプレ作り

---
## 0章 ブログのシステム構成について

### 静的サイトジェネレータについて
{{< figure src="/images/hugo-firebase-00/static-site-generator.png" alt="静的サイトジェネレータとは？" >}}

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
{{< figure src="/images/hugo-firebase-00/ssg-logos.png" alt="静的サイトジェネレータの種類" >}}

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
 
ブログ自動デプロイのシステム構成について、「静的サイトジェネレータ」について紹介しました！
0章では、各ツールを順番に役割、種類などを紹介していきます！

次回：[静的ホスティングについて≫](../hugo-firebase-02)

もし記事の内容でわからないところがあったら、★[質問箱](https://peing.net/ja/climbing_nacco?event=0)★に質問をください！Twitterで回答できるかはわかりませんが、このブログの記事かインフラ勉強会での発表の糧にさせていただきます。