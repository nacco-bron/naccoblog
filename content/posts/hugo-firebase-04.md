---
title: "Hugo+Firebase+GitHub+CircleCIでブログ作る-0_4"
slug: "hugo-firebase-04"
authors: ["nacco"]
date: "2018-09-23"
description: "実行ツールについて、0章まとめ"
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

こんにちは！[Nacco](https://twitter.com/climbing_nacco)です(｀･∀･´)ﾉ

[前前前前回](../hugo-firebase-00#システム構成)のシステム構成で使った各ツールを順番に役割、種類などを紹介していきます！

今回は「**実行ツールについて、0章まとめ**」です。

---

- **0章 ブログのシステム構成について**
    - [0章のねらい、システム構成について](../hugo-firebase-00)
    - [静的サイトジェネレータについて](../hugo-firebase-01)
    - [静的ホスティングについて](../hugo-firebase-02)
    - [変更管理ツールについて](../hugo-firebase-03)
    - **実行ツールについて、0章まとめ**
- [1章 Hugoで生成した静的サイトをローカル環境でプレビュー](../hugo-firebase-10)
- [2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ](../hugo-firebase-20)
- 3章 Hugoで生成した静的サイトをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ
- 4章 テーマ選び、記事テンプレ作り

---
## 0章 ブログのシステム構成について

### 実行ツールについて
{{< figure src="/images/hugo-firebase-00/citools-logos.png" alt="実行ツールについて" >}}

| 種類                                                      | 特徴                                                                                                                                                                               |
| :-------------------------------------------------------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[CircleCI](https://circleci.com/)**                     | GitHubとの連携が強い。ビルド時間を短くするためのキャッシュの読み取り/保存の特別なステップがある。わかりやすいUI。                                                                  |
| **[Wercker](www.wercker.com/)**                           | Travis CIやCircleCIと使用感は似ている。Orace Cloud上のk8sとの連携が簡単                                                                                                            |
| **[TravisCI](https://travis-ci.org/)**                    | WerckerやCircleCIと使用感は似ている。異なるバージョンの言語とパッケージでテストを実行するBuild matrixというツールがある。                                                          |
| **[Cloud Build](https://cloud.google.com/cloud-build/)**  | 1日あたり120分のビルドが無料。並行処理自体に課金はなく10個同時まで利用可能。docker pull/pushが高速。1ステップ1コンテナ使用する。pushトリガーだけでなくローカルからコマンド実行可。 |
| **[AWS CodeBuild](https://aws.amazon.com/jp/codebuild/)** | AWSの多数のサービスに連携可能。IAM権限設定ができる。実行環境にスペックが必要な時など有用。                                                                                           |


Dockerなどの**コンテナを使ってビルドやデプロイを実行するツール**です。

実行ツールはGitHubなどの変更管理ツールからpushを受けると次のような処理を実行します。

1. コンテナ上で静的サイトジェネレータをビルドして、HTMLなど静的サイトを生成する
2. コンテナ上で静的ホスティングのCLIをインストールし、デプロイコマンドを実行する

今回もCI/CDツールと呼んでいないのは、ビルド・デプロイ実行ツールとしてのみ利用しているからです。[Jenkins](https://jenkins.io/)はその用途では高機能すぎるので、選択肢からは外しました。[CircleCI](https://circleci.com/)を選びました。

Cloud Buildだけ使い勝手が少し違うようなので、余裕が出たら触ってみたいです。

今回CircleCI設定の際に、**ローカルとリモートでの実行結果の差異が発生するトラブル**が起きてトラシュー作業が大変勉強になりました！

トラシューの内容は、3章に書く予定です！

### 0章のまとめと初投稿の反省。
{{< figure src="/images/hugo-firebase-00/ending-00.png" alt="まとめと初投稿の反省。" >}}

以上、0章ではブログ自動デプロイのシステム構成と構成要素となった各ツールの役割、種類を紹介しました！
実際の構築手順は次回1章から書いていきます。

単純に「ブログを書きたい」「記事をシェアしたい」だけならば、はてなブログなど無料ブログサービスも選択肢として十分アリです。(というかそちらが王道ですね。)
調べていく中で「モバイル環境でもストレスがないこと」「コードで管理すること」「自動化しやすいこと」が静的サイトジェネレータで構築するWEBシステムには集結していて、技術トレンドを知る上で、やって良かった！と思うところです。

ここからは反省点。

実は、3章までの**構築作業自体は3時間弱ほどで完了**しています。

オンライン勉強会コミュニティで構築作業のナビゲータをやってくださった方がいたので短時間で構築できましたが、
記事執筆については全然捗らず…**0章だけで40時間は使っています**(´・ω・`;)

もう少し軽快に記事書いていけるように「HackMDのテンプレを作った話」もまたの機会に記事にします！
最後までお付き合いいただき、ありがとうございました！

～Hugo+Firebase+GitHub+CircleCIでブログ作るシリーズ～

次回:[1章 Hugoで生成した静的サイトをローカル環境でプレビュー](../hugo-firebase-10)

もし記事の内容でわからないところがあったら、★[質問箱](https://peing.net/ja/climbing_nacco?event=0)★に質問をください！Twitterで回答できるかはわかりませんが、このブログの記事か発表の糧にさせていただきます。

### 参考資料
- [静的サイトジェネレーター比較「StaticGen」](https://www.staticgen.com/)
- [【git】GitHubにpushしたらブランチ毎に自動デプロイする仕組みをさくらVPS上に作ってみた](https://y-hilite.com/2812/)
- [Now でクラウドの複雑さから解放されよう、今すぐに](https://qiita.com/aggre/items/f0cb9f8b8e8c54768e50)
- [Static Site Generators | DEVOPEDIA](https://devopedia.org/static-site-generators)
- [Bitbucketのプライベートリポジトリで管理しているJekyllで作った静的サイトを、無料のCIサービスwerckerを使って自動でS3にデプロイする](http://katahirado.hatenablog.com/entry/2014/04/27/131431)
- [Now + GitHub: Push to Deploy, Master Aliasing, Instant Rollbacks](https://zeit.co/blog/every-push-now)
- [人気CIツール比較まとめ【2015年12月版】](https://qiita.com/hiro_koba_jp/items/282e3b2e534f4bc22d64)
- [How to host Hugo static website generator on AWS Lambda](http://bezdelev.com/post/hugo-aws-lambda-static-website/)
- [S3/CloudFront/Route53/ACM構成の静的サイトをTerraformで構築 & CircleCIで自動デプロイ](https://tech.lucheholdings.com/entry/2018/09/25/220855)
- [継続的デリバリー ツールの統合](https://cloud.google.com/container-registry/docs/continuous-delivery?hl=ja)
- [エンジニアリングブログのCI環境をCodePipeline・CodeBuildに移行しました。](https://blog.mwed.info/posts/change_ci.html)
- [Google Cloud Buildとは一体何者なのか](https://swet.dena.com/entry/2018/08/20/170836)

### 勉強会 関連
- [HugoのサイトをCircle CIを使ってFirebaseにデプロイする](https://github.com/inductor/inductor.me)(indoctorさん:Hugoリポジトリ)
- [inductor/hugo-firebase-tools](https://github.com/inductor/hugo-firebase-tools/blob/master/Dockerfile)(indoctorさん:FirebaseCLI込みHugoビルドツール)
- [Firebase Hostingにブログを移行した](https://blog.haramishio.xyz/entry/migrate-firebase-hosting)(Morixさん:個人ブログ記事)
