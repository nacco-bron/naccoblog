---
title: "Hugo+Firebase+GitHub+CircleCIでブログ作る-03"
slug: "hugo-firebase-03"
authors: ["nacco"]
date: "2018-09-22"
description: "変更管理ツールについて"
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

お疲れ様です！[Nacco](https://twitter.com/climbing_nacco)です( ˘ω˘ )

[前前前回](../hugo-firebase-00#システム構成)のシステム構成で使った各ツールを順番に役割、種類などを紹介していきます！

今回は「**変更管理ツールについて**」調べた内容です。

---

- **0章 ブログのシステム構成について**
    - 0章のねらい、システム構成について
    - 静的サイトジェネレータについて
    - 静的ホスティングについて
    - **変更管理ツールについて**
    - 実行ツールについて、0章まとめ
- 1章 Hugoで生成したHTMLをローカル環境にデプロイ、表示
- 2章 Hugoで生成したHTMLをFirebase Hostingに手動デプロイ
- 3章 Hugoで生成したHTMLをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ
- 4章 テーマ選び、記事テンプレ作り

---
## 0章 ブログのシステム構成について

### 変更管理ツールについて
{{< figure src="/images/vcs-logos.png" alt="変更管理ツールについて" >}}

| 種類                                    | 特徴                                                                                                                                   |
| :-------------------------------------: | -------------------------------------------------------------------------------------------------------------------------------------- |
| **[GitHub](https://github.com/)**       | 登録リポジトリ数が圧倒的最多。OSSの公開リポジトリも多数ある。チームでの開発に使いやすいUI。                                            |
| **[GitLab](https://about.gitlab.com/)** | 非公開リポジトリが無料で作成できる。オープンソース、オンプレ運用も無料。CI/CD機能にGitHubやBitBucketなど外部のリポジトリが設定できる。 |
| **[Bitbucket](https://bitbucket.org/)** | 非公開リポジトリは5人共有までは無料。組込みのCI/CDツールや視覚化ツール、JiraやTrello、Slackの連携などができる。                        |

共通して**WEB上でGitリポジトリを作成、管理**できます。今回「バージョン管理システム(VCS)」と呼んでいないのは、**コンテンツの変更(push)をトリガーにしてビルド・デプロイ実行をキックするWebhookの役割**として使用しているからです。

[GitHub](https://github.com/)を選択しました。今回私は、はじめて[公開リポジトリ](https://github.com/nacco-bron/naccoblog)を作りました…！がんばって[草生やします](https://qiita.com/sta/items/2c1f0252a6a9ce5e2087)！！

基本的に、私が個人的に扱うソースコードは公開しておいて良いものなので、GitHubを使います。GitLabやBitbucketは非公開リポジトリが無料で作成できます！

ブログ自動デプロイのシステム構成について、「変更管理ツール」について紹介しました！
0章では、各ツールを順番に役割、種類などを紹介していきます！

次回：[実行ツールについて≫](../hugo-firebase-04)