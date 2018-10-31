---
title: "Hugo+Firebase+GitHub+CircleCIでブログ作る-0_2"
slug: "hugo-firebase-02"
authors: ["nacco"]
date: "2018-09-21"
description: "静的ホスティングについて"
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

おはようございます！[Nacco](https://twitter.com/climbing_nacco)です(｢・ω・)｢

[前々回](../hugo-firebase-00#システム構成)のシステム構成で使った各ツールを順番に役割、種類などを紹介していきます！

今回は「**静的ホスティングについて**」調べた内容です。

---

- **0章 ブログのシステム構成について**
    - [0章のねらい、システム構成について](../hugo-firebase-00)
    - [静的サイトジェネレータについて](../hugo-firebase-01)
    - **静的ホスティングについて**
    - [変更管理ツールについて](../hugo-firebase-03)
    - [実行ツールについて、0章まとめ](../hugo-firebase-04)
- [1章 Hugoで生成した静的サイトをローカル環境でプレビュー](../hugo-firebase-10)
- 2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ
- 3章 Hugoで生成した静的サイトをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ
- 4章 テーマ選び、記事テンプレ作り

---
## 0章 ブログのシステム構成について

### 静的ホスティングについて
{{< figure src="/images/hugo-firebase-00/shosting-logos.png" alt="静的ホスティングについて" >}}

各サービス出自が違うので、得意分野が異なりますが、基本的に**CLIツール**は必ずついています。

| 種類                                                                                                                                                                              | 特徴                                                                                                                                                                                                                                                                                     |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[Now](https://zeit.co/now)**                                                                                                                                                    | デプロイの統合CLI環境。「now」の3文字コマンドで手動デプロイ。[GitHub App](https://github.com/apps/now)をGitHubリポジトリにインストールすると、masterブランチにgit pushするごとにNowに自動デプロイできる。mergeのpull requestの承認でもデプロイ可。revertでロールバックもできる。すごい！ |
| **[Firebase Hosting](https://firebase.google.com/docs/hosting/?hl=ja)**                                                                                                           | CLIで簡単にinit/deployできる。モバイル向けBackend(MBaaS)を提供するFirebaseの一機能。                                                                                                                                                                                                     |
| **[AWS S3](https://aws.amazon.com/jp/s3/)**                                                                                                                                       | WEBサイトホスティング用に設定して利用可能な[クラウドオブジェクトストレージ](https://aws.amazon.com/jp/what-is-cloud-object-storage/)。WAFを作成してCloudFrontに適用など、AWSはとにかくサービスが多彩なので連携して利用できる。                                                           |
| **[Netlify](https://www.netlify.com/)**                                                                                                                                           | 静的ホスティング専用サービス。ドラッグ＆ドロップでのデプロイやCLI操作も可能。GitHubなどのGitリポジトリと連携。HTTP/2による配信に対応。                                                                                                                                                   |
| **[GitHub Pages](https://pages.github.com/)**                                                                                                                                     | gh-pagesブランチを作成し、git pushすることでデプロイ。変更管理ツール、実行ツールを包括。                                                                                                                                                                                                 |
| **[GitLab Pages](https://about.gitlab.com/features/pages/)**                                                                                                                      | GitHub Pagesと同様のサービス。プライベートリポジトリも無料で作成できる！                                                                                                                                                                                                                 |
| **[Bitbucket](https://confluence.atlassian.com/bitbucket/publishing-a-website-on-bitbucket-cloud-221449776.html#PublishingaWebsiteonBitbucketCloud-ConfigureaWebsiteforHosting)** | GitHub、GitLab同様Hostingサービスがあるところまでは確認できましたが、それ以上は今回調査不足…使っている方、教えてください！                                                                                                                                                               |

**Netlify**はビルドインされている静的サイトジェネレータが多数あり、ボタンひとつでGitHubリポジトリを作成、サイトを構築できます。**Firebase Hosting**や**AWS S3**は多機能なマネージドサービスの一機能として静的ホスティングを提供しています。

**Now**、**Netlify**、**GitHub Pages**、**GitLab Pages**はgit pushをトリガーにして自動デプロイができるので、今回のシステム構成のステップをいくつか省略できます！

今回はステップごとにツールを把握したかったので、ホスティングサービス単体で[Firebase Hosting](https://firebase.google.com/docs/hosting/?hl=ja)を選択しました。
シングルページアプリケーション(SPA)の開発に興味があってFirebaseを選びましたが、静的サイトジェネレータを利用する場合、コンテンツの移行が容易なので、料金形態や使用感で引っ越しも検討します。

**Now**とGitHubの連携が強いなあと思って注目しています。

<blockquote class="twitter-tweet" data-lang="ja"><p lang="en" dir="ltr">Now + GitHub: Evolved<br><br>⬩ Automatic deployments for every single push<br>⬩ Automatic alias of master branch to custom domain(s)<br>⬩ Instant rollbacks when reverting<a href="https://t.co/rCrY2VoxDi">https://t.co/rCrY2VoxDi</a> <a href="https://t.co/5Z53YOwPlc">pic.twitter.com/5Z53YOwPlc</a></p>&mdash; ZEIT (@zeithq) <a href="https://twitter.com/zeithq/status/1041734451691175937?ref_src=twsrc%5Etfw">2018年9月17日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

ブログ自動デプロイのシステム構成について、「静的ホスティング」について紹介しました！
0章では、各ツールを順番に役割、種類などを紹介していきます！

次回：[変更管理ツールについて≫](../hugo-firebase-03)

もし記事の内容でわからないところがあったら、★[質問箱](https://peing.net/ja/climbing_nacco?event=0)★に質問をください！Twitterで回答できるかはわかりませんが、このブログの記事かインフラ勉強会での発表の糧にさせていただきます。
