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
draft: false
cover: /images/hugo-firebase-20/hugo-firebase-20.png
---

おひさしぶりです！[Nacco](https://twitter.com/climbing_nacco)ですᕕ(´ ω` )ᕗ

[1章](../hugo-firebase-10)からHugoでこのブログを構築する**実際の手順**を書いています！

本章から生成した静的サイトを**静的ホスティングFirebase Hostingにデプロイ**していきますよー！

---

- [0章 ブログのシステム構成について](../hugo-firebase-00)
- [1章 Hugoで生成した静的サイトをローカル環境でプレビュー](../hugo-firebase-10)
- **2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ**
  - **2章で得られること、Firebaseプロジェクトの作成**
  - **Firebase CLIのセットアップ、Hostingの初期化、Hostingへの手動デプロイ** 
- 3章 Hugoで生成した静的サイトをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ
- 4章 テーマ選び、記事テンプレ作り

---
## 2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ

### 2章で得られること

- Firebaseプロジェクトの作成手順
- Firebase CLIによるHosting用のディレクトリ初期設定の方法
- Hostingへの手動デプロイ手順

### Firebaseプロジェクトの作成
{{< figure src="/images/hugo-firebase-20/fb-project.png" alt="Firebaseプロジェクトの作成" >}}

[Firebaseの管理パネル](https://console.firebase.google.com)にGoogleアカウントでログインします。

「プロジェクトを追加」をクリックして**プロジェクト名**と**地域/ロケーション**を設定してください。

ここではプロジェクト名を「naccoblog」アナリティクスの地域を「日本」
Cloud Firestoreのロケーションを「asia-northeast1」とします。

Firebaseプロジェクトの作成ができると「naccoblog」Project Overviewのページ※1が表示されます。

左メニュー 「開発」から**Hosting**を選択し、「開始方法」をクリックすると[手順](https://firebase.google.com/docs/cli/?authuser=0)が表示されるので、それに従ってFirebase CLIのセットアップをします。

{{% text s="0.8" color="#7058a3" %}}
**※1** [Sparkプラン](https://firebase.google.com/pricing/?authuser=0)はHotingで1GBの保存と月10GBのファイル転送までが無料です。静的ホスティングの用途でこれを超えることはあまりありませんが、それ以上ですと有料になるのでご注意ください！
{{% /text %}}

### Firebase CLIのセットアップ
{{< figure src="/images/hugo-firebase-20/fb-logined.png" alt="Firebase CLIのセットアップ" >}}

FirebaseのCLI「firebase-tools」を使って、コマンド操作ができるようにセットアップします。

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
v12.2.0
```

Node.jsのパッケージマネージャnpmも一緒にインストールされるので確認

```
C:\Hugo\naccoblog>npm --version
6.9.0
```
#### Firebase CLIのインストール

[Firebase CLI リファレンス](https://firebase.google.com/docs/cli/?authuser=0)の手順通りに`npm install`コマンドでFirebase CLIをインストールします。

```
C:\Hugo\naccoblog>npm install -g firebase-tools
```
{{% text s="0.8" color="#7058a3" %}}
**※2** 「**-g**」オプションはグローバルインストールのオプションで、どのディレクトリからもパッケージが利用できるようにします。
{{% /text %}}


### Firebaseプロジェクトにログイン

Hugoプロジェクト(ブログ)のディレクトリでFirebaseプロジェクトにログインします。

コマンドプロンプトから`firebase login`コマンドを実行します。
```
C:\Hugo\naccoblog>firebase login
? Allow Firebase to collect anonymous CLI usage and error reporting information? (Y/n) Y
```
エラーレポートをGoogleに送信するかを選択します。(お好みで)

Node.jsからWEBへのアクセスがありファイアウォールの警告がでたら、許可してください。

WEBブラウザでGoogleアカウントにログインするとWEBブラウザとコマンドプロンプトにログイン成功のメッセージが表示されます。

```

Visit this URL on any device to log in:
https://accounts.google.com/o/oauth2/auth?client_id=
Waiting for authentication...

+  Success! Logged in as [google account]

```
### Firebaseプロジェクトディレクトリの初期設定

Hugoプロジェクト(ブログ)のディレクトリで`firebase init`コマンドを実行します。
```
C:\Hugo\naccoblog>firebase init

     ######## #### ########  ######## ########     ###     ######  ########
     ##        ##  ##     ## ##       ##     ##  ##   ##  ##       ##
     ######    ##  ########  ######   ########  #########  ######  ######
     ##        ##  ##    ##  ##       ##     ## ##     ##       ## ##
     ##       #### ##     ## ######## ########  ##     ##  ######  ########

You're about to initialize a Firebase project in this directory:

  C:\Hugo\naccoblog

Before we get started, keep in mind:

  * You are currently outside your home directory

? Are you ready to proceed? (Y/n)Y
```
続行するか聞かれるので「Y」(はい)と入力してください。

どのサービスの設定をするかを選びます。
ここでは「Hosting」を選択します(↓キーで移動、スペースキーで選択、Enterキーで決定)
```
? Which Firebase CLI features do you want to set up for this folder? Press Spac
e to select features, then Enter to confirm your choices. (Press <space> to sel
ect)
>( ) Database: Deploy Firebase Realtime Database Rules
 ( ) Firestore: Deploy rules and create indexes for Firestore
 ( ) Functions: Configure and deploy Cloud Functions
 ( ) Hosting: Configure and deploy Firebase Hosting sites
 ( ) Storage: Deploy Cloud Storage security rules
```

連携するFirebaseプロジェクトを選択します。

[Firebaseの管理パネル](https://console.firebase.google.com)で作成したプロジェクト名を選択します。(↓キーで移動、Enterキーで選択)
```
Hosting: Configure and deploy Firebase Hosting sites

=== Project Setup

First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add,
but for now we'll just set up a default project.

? Select a default Firebase project for this directory: (Use arrow keys)
> [don't setup a default project]
  naccoblog (naccoblog)
  [create a new project]
```

公開ディレクトリ(Firebase Hostingにデプロイするディレクトリ)を設定します。

デフォルトで「public」ディレクトリになっています。Hugoのコンテンツもpublicディレクトリ内に生成されるので、そのままEnterキーで確定します。
```
? Select a default Firebase project for this directory: naccoblog (naccoblog)
i  Using project naccoblog (naccoblog)

=== Hosting Setup

Your public directory is the folder (relative to your project directory) that
will contain Hosting assets to be uploaded with firebase deploy. If you
have a build process for your assets, use your build's output directory.

? What do you want to use as your public directory? (public)

```

サイト構成の確認があります。

SPA(single-page app)ですか？と聞かれるので、今回は「N」(いいえ)と入力してください。
```
? What do you want to use as your public directory? public
? Configure as a single-page app (rewrite all urls to /index.html)? (y/N)N
```

すでにHugoでコンテンツ生成を行っている場合は、`public/404.html`と`public/index.html`が生成されているので、上書きしないでください。「N」を入力します。

```
? File public/404.html already exists. Overwrite? N
i  Skipping write of public/404.html
? File public/index.html already exists. Overwrite? N
i  Skipping write of public/index.html

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...
i  Writing gitignore file to .gitignore...

+  Firebase initialization complete!
```
これでFirebaseプロジェクト(ディレクトリ)の初期設定が完了しました。

Hugoプロジェクト内に`.firebaserc`と`firebase.json`が作成されます。

{{< figure src="/images/hugo-firebase-20/fb-init.png" alt="firebase files" >}}

### Firebase Hosting への手動デプロイ

Hugoでコンテンツを生成し、Firebase CLIのコマンドでHostingへデプロイします。
(変更管理:GitHubへのpushの際にHostingへ自動でデプロイする方法は3章で説明します。)

#### コンテンツの生成

`hugo`コマンドを実行し、markdown(.md)からコンテンツを生成します。

```
C:\Hugo\naccoblog>hugo
Building sites …
                   | JA | EN
+------------------+----+----+
  Pages            | 37 | 14
  Paginator pages  |  0 |  0
  Non-page files   |  0 |  0
  Static files     | 46 | 46
  Processed images |  0 |  0
  Aliases          |  8 |  1
  Sitemaps         |  2 |  1
  Cleaned          |  0 |  0

Total in 295 ms
```

[前章](../hugo-firebase-10)では`hugo server`コマンドでメモリ上にコンテンツを展開しましたが、`hugo`コマンドはディスクにコンテンツを展開(格納)します。

publicディレクトリ以下にコンテンツのファイル群が生成されているはずです。
{{< figure src="/images/hugo-firebase-20/contents.png" alt="public contents" >}}

#### Hosting へデプロイ

これらを`firebase deploy`コマンドでHostingへアップします。

```
C:\Hugo\naccoblog>firebase deploy

=== Deploying to 'naccoblog'...

i  deploying hosting
i  hosting[naccoblog]: beginning deploy...
i  hosting[naccoblog]: found 27 files in public
+  hosting[naccoblog]: file upload complete
i  hosting[naccoblog]: finalizing version...
+  hosting[naccoblog]: version finalized
i  hosting[naccoblog]: releasing new version...
+  hosting[naccoblog]: release complete

+  Deploy complete!

Project Console: https://console.firebase.google.com/ ...
Hosting URL: https://naccoblog.firebaseapp.com
```
Hosting URL が表示されるので、アクセスして確認します。

{{< figure src="/images/hugo-firebase-20/fb-hosting-done.png" alt="Hostingへのデプロイ" >}}

### 2章のまとめ
{{< figure src="/images/hugo-firebase-20/ending-20.png" alt="コマンドでぽんっ！！" >}}

やっとサイトがアップできました！もちろんHTTPSに対応していますよ！
本章ではgitを絡めず、直接Hostingへデプロイしました。
これだけでも、かなりシンプルな操作で更新が完了します。

次章ではGitHubとCircleCIを連携して、markdownの変更をコミットしたら
自動でHostingにコンテンツがデプロイされる仕組みを作ってみましょう！

次回：3章 Hugoで生成した静的サイトをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ

もし記事の内容でわからないところがあったら、★[質問箱](https://peing.net/ja/climbing_nacco?event=0)★に質問をください！Twitterで回答できるかはわかりませんが、このブログの記事か発表の糧にさせていただきます。

### 参考資料
- [Firebase Hosting を使ってみる | Firebase](https://firebase.google.com/docs/hosting/quickstart?hl=ja)
- [Firebase CLI リファレンス | Firebase](https://firebase.google.com/docs/cli/?authuser=0)