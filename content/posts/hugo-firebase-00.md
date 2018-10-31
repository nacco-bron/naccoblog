---
title: "Hugo+Firebase+GitHub+CircleCIでブログ作る-0_0"
slug: "hugo-firebase-00"
authors: ["nacco"]
date: "2018-09-19"
description: "0章のねらい、システム構成について"
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

はじめまして！[Nacco](https://twitter.com/climbing_nacco)です。＼＼ ٩( 'ω' )و ／／

このブログを構築するまでの流れを最初の記事にすることにしました！

**Hugo(静的サイトジェネレータ)**で構築しています。

序章(この章)＋4章に分けて書いていきます！

---
- **0章 ブログのシステム構成について**
    - **0章のねらい、システム構成について**
    - [静的サイトジェネレータについて](../hugo-firebase-01)
    - [静的ホスティングについて](../hugo-firebase-02)
    - [変更管理ツールについて](../hugo-firebase-03)
    - [実行ツールについて、0章まとめ](../hugo-firebase-04)
- [1章 Hugoで生成した静的サイトをローカル環境でプレビュー](../hugo-firebase-10)
- 2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ
- 3章 Hugoで生成した静的サイトをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ
- 4章 テーマ選び、記事テンプレ作り

---
## 0章 ブログのシステム構成について

### 0章のねらい

静的サイトジェネレータで構築するブログのシステム構成、各サービスがどういうものなのかを理解して、最終的には「**同種サービスの別構成も考えられる**」ようになるのがねらいです。


### システム構成 (実装)

最終的に実装したシステム構成がこちらです。
0章では実装したツールについて調べて、システム構成を**汎化**していくことにしました。

- **Hugo**
- **Firebase Hosting**
- **GitHub**
- **CircleCI**

{{< figure src="/images/hugo-firebase-00/kousei2.png" alt="ブログ自動デプロイのシステム構成" >}}

私が所属しているオンライン勉強会コミュニティ「[インフラ勉強会](https://wp.infra-workshop.tech/)」で「**Hugo**」というもので「**爆速でブログが立てられる**」「**勝手にHTTPS化してくれる**※1」ということを小耳にはさみ、私も早速使ってみようと思いました。

「**ついでに自動デプロイの設定しとくといいよ**」ということでした。なるほど、わからない。

まだ、この時は上の図のような流れは見えてみません。

{{% text s="0.8" color="#7058a3" %}}
**※1** 後で正しくはFirebase Hostingの機能だと知りました
{{% /text %}}

### システム構成

いろいろと調べた結果、**静的サイトジェネレータ**で構築する「**ブログ自動デプロイのシステム**」はこうだ！とわかりました。

- **静的サイトジェネレータ** Hugo
- **静的ホスティング** Firebase Hosting
- **変更管理ツール** GitHub
- **実行ツール** CircleCI


{{< figure src="/images/hugo-firebase-00/kousei.png" alt="ブログ自動デプロイのシステム構成" >}}

ブログ自動デプロイについて、最終結果「システム構成」から紹介しました。
次の記事から、各ツールを順番に役割、種類などを紹介していきます！

次回：[静的サイトジェネレータについて≫](../hugo-firebase-01)

もし記事の内容でわからないところがあったら、★[質問箱](https://peing.net/ja/climbing_nacco?event=0)★に質問をください！Twitterで回答できるかはわかりませんが、このブログの記事かインフラ勉強会での発表の糧にさせていただきます。