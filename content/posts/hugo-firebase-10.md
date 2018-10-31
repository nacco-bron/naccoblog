---
title: "Hugo+Firebase+GitHub+CircleCIでブログ作る-1_0"
slug: "hugo-firebase-10"
authors: ["nacco"]
date: "2018-09-24"
description: "1章で得られること、Hugoのインストール"
# Catergory Tech Social Life
categories:
- Tech
tags:
- Hugo
- Firebase
- GitHub
- CircleCI
draft: false
cover: /images/hugo-firebase-10/hugo-firebase-10.png
---

おはようございます！[Nacco](https://twitter.com/climbing_nacco)です(｀・ω・´)

このブログを構築するまでの流れを記事にすることにしました！

[0章](../hugo-firebase-00)では、システム構成について見ていきました。この章からは**実際の手順**について書いていきますよー！

---

- [0章 ブログのシステム構成について](../hugo-firebase-00)
- **1章 Hugoで生成した静的サイトをローカル環境でプレビュー**
  - **1章で得られること、Hugoのインストール**
  - [Hugoプロジェクトの作成、初期化、テーマの設定](../hugo-firebase-11)
  - [コンテンツの作成、ローカルでプレビュー、1章まとめ](../hugo-firebase-12)
- 2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ
- 3章 Hugoで生成した静的サイトをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ
- 4章 テーマ選び、記事テンプレ作り

---
## 1章 Hugoで生成した静的サイトをローカル環境でプレビュー

### 1章で得られること

- Hugoのセットアップ手順
- Hugoのローカル環境でのサーバ起動＆表示確認手順

さっそくHugoのインストールからやっていきますよ！

### Hugoのセットアップ
{{< figure src="/images/hugo-firebase-10/hugo-install.png" alt="Hugoのインストール" >}}

#### 作業環境
- Windows10 マシン
- Git for Windows 2.18.0
  - [このあたりを見てセッティング](http://vdeep.net/git-for-windows)

---
#### Hugoのインストール

Hugoの公式ドキュメントで2つの方法が紹介されていました。

1. **[Hugoのバイナリを設置する](https://gohugo.io/getting-started/installing#binary-cross-platform)**
2. **[パッケージマネージャでインストールする](https://gohugo.io/getting-started/installing#chocolatey-windows)**

Hugo本体はたった1つのバイナリ(hugo.exe)なので、それを任意のフォルダに設置するだけで、インストール完了です。

ただ、動作環境に合ったものを探しに行ったりするのが面倒だし、せっかくなので2.の**パッケージマネージャ**を使っていこうと思います！


パッケージマネージャ[Chocolatey](https://chocolatey.org/)を使用します。
{{< figure src="/images/hugo-firebase-10/chocolatey-icon.png" alt="Chocolatey">}}
##### Chocolateyのインストール

PowerShellを**管理者権限**で起動します。
{{< figure src="/images/hugo-firebase-10/powershelladmin.png" alt="管理者として実行" >}}

PowerShellの下記コマンド※1で**Chocolateyインストールのシェルスクリプトを実行**します。
```
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

{{% text s="0.8" color="#7058a3" %}}
**※1** インターネット上のシェルスクリプトを実行するために、一時的に実行ポリシーを変更しています。「**Bypass**」はインターネットからダウンロードされた署名されていないスクリプトなども何もブロックされず、警告もメッセージも表示しないポリシー「**-Scope Process**」でプロセスにのみ適用「**-Force**」で警告を含む全てのプロンプト表示を抑制
{{% /text %}}

**インストール確認**

`choco`コマンドでバージョンが表示されていたらインストールできています。
```
PS C:\WINDOWS\system32> choco
Chocolatey v0.10.11
Please run 'choco -?' or 'choco <command> -?' for help menu.
```

##### Hugoのインストール(choco install)

`choco install`コマンドで**Hugoをインストール**します。
```
choco install hugo --confirm
```
{{% text s="0.8" color="#7058a3" %}}
**※2** 「**--confirm**」オプションは「**-y**」と書き換えてもOKです。全ての確認に「はい」と応答するオプションです。
{{% /text %}}

こんな感じで進んでいきます。
```
Chocolatey v0.10.11
Installing the following packages:
hugo
By installing you accept licenses for the packages.
Progress: Downloading hugo 0.47.1... 100%

hugo v0.47.1 [Approved]
hugo package files install completed. Performing other installation steps.
The package hugo wants to run 'chocolateyInstall.ps1'.
Note: If you don't run this script, the installation will fail.
Note: To confirm automatically next time, use '-y' or consider:
choco feature enable -n allowGlobalConfirmation
Do you want to run the script?([Y]es/[N]o/[P]rint):Y

```
**Hugoの 最新版**※2のダウンロードとインストールがはじまります。

{{% text s="0.8" color="#7058a3" %}}
**※3** あとで実行ツール(CircleCI)のコンテナ上でHugoをインストールして使う時は**Hugoのバージョンを固定**するようなDockerfileを使いたいと思います。
{{% /text %}}

```
Downloading hugo 64 bit
  from 'https://github.com/spf13/hugo/releases/download/v0.47.1/hugo_0.47.1_Windows-64bit.zip'
Progress: 100% - Completed download of C:\WINDOWS\system32\onfig\hugo\0.47.1\hugo_0.47.1_Windows-64bit.zip (17.96 MB).
Download of hugo_0.47.1_Windows-64bit.zip (17.96 MB) completed.
Hashes match.
Extracting C:\WINDOWS\system32\onfig\hugo\0.47.1\hugo_0.47.1_Windows-64bit.zip to C:\ProgramData\chocolatey\lib\hugo\tools...
C:\ProgramData\chocolatey\lib\hugo\tools
 The install of hugo was successful.
  Software installed to 'C:\ProgramData\chocolatey\lib\hugo\tools'

Chocolatey installed 1/1 packages.
 See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
```

`Software installed to 'C:\ProgramData\chocolatey\lib\hugo\tools'`

と、**Hugoがインストールされた場所**が表示されます。クリップボードなどに控えてください。

##### Hugoのpathを通します。

Hugoの[pathを通して](https://qiita.com/sta/items/63e1048025d1830d12fd)、どこからでもhugoコマンドを実行できるようにします。

{{< figure src="/images/hugo-firebase-10/hugo-env-var.png" alt="環境変数を編集" >}}
［コントロールパネル］→［システムとセキュリティ］→［システム］→［**システムの詳細設定**］→ ［**環境変数**］

「ユーザ環境変数」の 「**Path**」 をダブルクリック

新規作成で

`C:\ProgramData\chocolatey\lib\hugo\tools`を登録してOKボタンをクリック

環境変数の追加が終わったら、念のためPowerShellを再起動します。

**Hugoのpathが通ったことの確認**
```
PS C:\Users\nacco> hugo version
Hugo Static Site Generator v0.47.1 windows/amd64 BuildDate: 2018-08-20T08:17:26Z
```

**Hugoのインストールが完了です！**

それでは、Hugoのプロジェクトを作っていきます。Gitでのバージョン管理にも少し触れます。

次回：[Hugoプロジェクトの作成、初期化≫](../hugo-firebase-11)

もし記事の内容でわからないところがあったら、★[質問箱](https://peing.net/ja/climbing_nacco?event=0)★に質問をください！Twitterで回答できるかはわかりませんが、このブログの記事かインフラ勉強会での発表の糧にさせていただきます。