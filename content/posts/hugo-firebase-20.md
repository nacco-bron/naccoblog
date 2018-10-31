---
title: "Hugo+Firebase+GitHub+CircleCIでブログ作る-2_0"
slug: "hugo-firebase-20"
authors: ["nacco"]
date: "2018-10-20"
description: "2章で得られること、Firebase プロジェクトの作成"
# Catergory Tech Social Life
categories:
- Tech
tags:
- Hugo
- Firebase
- GitHub
- CircleCI
draft: true
cover: /images/hugo-firebase-10/hugo-firebase-20.png
---

おひさしぶりです！[Nacco](https://twitter.com/climbing_nacco)ですᕕ(´ ω` )ᕗ

[1章](../hugo-firebase-10)からHugoでこのブログを構築する**実際の手順**を書いています！

本章から生成した静的サイトを**静的ホスティングFirebase Hostingにデプロイ**していきますよー！

---

- [0章 ブログのシステム構成について](../hugo-firebase-00)
- [1章 Hugoで生成した静的サイトをローカル環境でプレビュー](../hugo-firebase-10)
- **2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ**
  - 2章で得られること、Firebaseプロジェクトの作成
  - Firebase CLIのセットアップ、Hostingの初期化、Hostingへの手動デプロイ 
- 3章 Hugoで生成した静的サイトをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ
- 4章 テーマ選び、記事テンプレ作り

---
## 2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ

### 2章で得られること

- Firebaseプロジェクトの作成手順
- Firebase CLIによるHostingの初期化、Hostingへの手動デプロイ手順

### Firebaseプロジェクトの作成
{{< figure src="/images/hugo-firebase-20/create-firebase-project.png" alt="Firebaseプロジェクトの作成" >}}

[Firebaseの管理パネル](https://console.firebase.google.com)にGoogleアカウントでログインします。

「プロジェクトを追加」をクリックして**プロジェクト名**と**地域/ロケーション**を設定してください。

ここではプロジェクト名を「naccoblog」アナリティクスのロケーションを「日本※1」とします。

{{% text s="0.8" color="#7058a3" %}}
**※1** Cloud Firestore のロケーションはまだ日本リージョンがないので米国マルチリージョン(us-central)のままでOKです。
{{% /text %}}

Firebaseプロジェクトの作成ができると「naccoblog」Project Overviewのページ※2が表示されます。

左メニュー 「開発」から**Hosting**を選択し、「使ってみる」をクリックするとFirebase CLIのインストールコマンドが表示されるので、それに従ってFirebase CLIのセットアップをします。

{{% text s="0.8" color="#7058a3" %}}
**※2** [Sparkプラン](https://firebase.google.com/pricing/?authuser=0)はHotingで1GBの保存と月10GBのファイル転送までが無料です。静的ホスティングの用途でこれを超えることはあまりありませんが、それ以上ですと有料になるのでご注意ください！
{{% /text %}}

### Firebase CLIのセットアップ
{{< figure src="/images/hugo-firebase-20/settingup-firebase-cli.png" alt="Firebase CLIのセットアップ" >}}

#### Node.jsのインストール

Firebase CLIはNode.jsのパッケージなのでNode.jsをインストールします。

[前章](../hugo-firebase-10)に引き続きパッケージマネージャのChocolateyを使っていきます。
公式サイトのインストーラを使用しても、もちろん大丈夫です。

コマンドプロンプト(またはPowershell)を管理者権限で起動して`choco install`
```
C:\WINDOWS\system32>choco install -y nodejs
```

コマンドプロンプトを再起動しインストール確認

```
C:\Hugo\naccoblog>node --version
v10.11.0
```

Node.jsのパッケージマネージャnpmも一緒にインストールされるので確認

```
C:\Hugo\naccoblog>npm --version
6.4.1
```
#### Firebase CLIのインストール

ここでFirebase Hosting の管理パネルに戻って手順通りに`npm install`コマンドでFirebase CLIをインストールします。

```
C:\Hugo\naccoblog>npm install -g firebase-tools
```
{{% text s="0.8" color="#7058a3" %}}
**※3** 「**-g**」オプションはグローバルインストールのオプションで、どのディレクトリからもパッケージが利用できるようにします。
{{% /text %}}


#### Firebaseプロジェクトにログイン、初期設定

HugoプロジェクトのディレクトリでFirebase CLIを使ってFirebaseプロジェクトにログインします。
```
C:\Hugo\testblog>firebase login
? Allow Firebase to collect anonymous CLI usage and error reporting information? (Y/n) Y
```
```

Visit this URL on any device to log in:
https://accounts.google.com/o/oauth2/auth?client_id=
Waiting for authentication...

+  Success! Logged in as [googleaccount]]

```


### 参考資料
- [Firebase Hosting を使ってみる | Firebase](https://firebase.google.com/docs/hosting/quickstart?hl=ja)