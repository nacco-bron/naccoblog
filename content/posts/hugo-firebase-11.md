---
title: "Hugo+Firebase+GitHub+CircleCIでブログ作る-1_1"
slug: "hugo-firebase-11"
authors: ["nacco"]
date: "2018-09-25"
description: "Hugoプロジェクトの作成、初期化、テーマの設定"
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

こんにちは！[Nacco](https://twitter.com/climbing_nacco)です=͟͟͞͞⊂(⊂ 'ω')

このブログを構築した手順を記事にしているのです！

[前回](../hugo-firebase-00)Hugoインストールが完了したところから進めます。

---

- [0章 ブログのシステム構成について](../hugo-firebase-00)
- **1章 Hugoで生成した静的サイトをローカル環境でプレビュー**
  - [1章で得られること、Hugoのインストール](../hugo-firebase-10)
  - **Hugoプロジェクトの作成、初期化、テーマの設定**
  - [コンテンツの作成、ローカルでプレビュー、1章まとめ](../hugo-firebase-12)
- 2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ
- 3章 Hugoで生成した静的サイトをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ
- 4章 テーマ選び、記事テンプレ作り

---
## 1章 Hugoで生成した静的サイトをローカル環境でプレビュー

#### Hugoプロジェクトの作成、初期化
{{< figure src="/images/hugo-firebase-10/create-pj.png" alt="Hugoプロジェクトの作成、初期化" >}}

##### Hugoプロジェクトの作成
**Hugoプロジェクトのディレクトリ(フォルダ)を作成します。**

まずプロジェクトの格納元`C:\Hugo`ディレクトリを作成します。

##### Hugoプロジェクトの初期化

PowerShell(もしくはコマンドプロンプト※1)で`C:\Hugo`に移動

{{% text s="0.8" color="#7058a3" %}}
**※1** ターミナルソフトはお好みのものを使ってください。ファイル、ディレクトリ操作やgitコマンドが使えれば何でも良いです。ただし、WSL(Windows Subsystem for Linux)でやる場合はLinux環境でのgitやhugoのセットアップ手順になるので、このブログには書いてないです！
{{% /text %}}


`hugo new site`コマンド実行します。

`hugo new site [project]`


```
C:\Hugo>hugo new site naccoblog 

Congratulations! Your new Hugo site is created in C:\Hugo\naccoblog.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/, or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>\<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.
```

Hugoプロジェクト「naccoblog」のディレクトリができました(`･∀-´)☆

中身はこんな感じです。
{{< figure src="/images/hugo-firebase-10/hugo-directry.png" alt="Hugo Directory Structure" >}}

- **archetypes** … 新規コンテンツ作成時の原型`default.md`を置く場所
- **assets** … [Hugo Pipes](https://gohugo.io/hugo-pipes/introduction/)という新機能で利用する場所
- **content** … コンテンツ(.mdファイル)を置く場所
- **data** … Webサイト生成の設定ファイル(YAML、JSON、TOML)を置く場所
- **layouts** … テンプレート(ただしテーマを設定するとthemes\theme-nameディレクトリ下のlayoutsを参照)
- **static** … 静的コンテンツ(画像、CSS、JavaScriptなど)を置く場所。WEBサイト生成時にそのままコピーされる
- **config.toml** … Hugo本体の設定ファイル

---
#### テーマの設定
{{< figure src="/images/hugo-firebase-10/setting-theme.png" alt="テーマの設定" >}}

Hugoのテーマは
[https://themes.gohugo.io/](https://themes.gohugo.io/) から選べます！

このブログでは、「**[Minimo](https://github.com/MunifTanjim/minimo)**」を使いました！

##### テーマのダウンロード 

テーマのダウンロード方法は以下の2種類(gitを使います※3)

1. テーマのGitHubリポジトリを**git clone**する(Hugoプロジェクト内に単純にコピー)
2. テーマのGitHubリポジトリを**git submodule**にする

{{% text s="0.8" color="#7058a3" %}}
**※2** git環境構築については[作業環境](#%E4%BD%9C%E6%A5%AD%E7%92%B0%E5%A2%83)を参照。ここではローカルリポジトリ作成のみ手順に含めています。
{{% /text %}}

submodule化するとテーマのみを更新したりできますが、そういうことはしない予定なので今回は**1.の手順**にします。

**gitのローカルリポジトリの作成**

テーマを入れる前の状態を一度保存するため、ローカルリポジトリを作ります。
```
C:\Hugo>cd naccoblog
C:\Hugo\naccoblog>git init
```

**.gitignoreを作成します。**

リポジトリを作成したら、セットでやっておきます。**.gitignore**にgit管理から除外するファイルを書きます。

「コンテンツ」と「テンプレート」から生成された、実際にサーバにデプロイする「HTML群」は全て`public`ディレクトリ下に出力されます。
**gitでは源資となるものだけをgitで管理**し、生成データはgit管理から除外します。

```
C:\Hugo\naccoblog>echo /public/> .gitignore 
```
[エクスプローラ上で作る方法](https://qiita.com/sgur/items/745e0ee02c69b50bf9e5)もあります。

.gitignoreの内容
```git
/public/
```


**一度git commitしましょう。**

git commitは刻んでいけ！って[ばっちゃ](https://twitter.com/kazuhito_m)が言ってました。
```
C:\Hugo\naccoblog>git add . 
C:\Hugo\naccoblog>git commit -m "first commit!"
```

**テーマのリポジトリをgit cloneします。**
```
C:\Hugo\naccoblog>git clone --depth 1 https://github.com/MunifTanjim/minimo themes/minimo
```

git cloneしてきた場合は、**themes/theme-nameディレクトリ内の.gitディレクトリは削除しましょう。**(外部リポジトリの設定が混入すると想定しない動作をするため。)
{{< figure src="/images/hugo-firebase-10/del-git-dir.png" alt="テーマをローカルリポジトリで管理" >}}

ここでも**git commit**しておくと良いですよ！
```
C:\Hugo\naccoblog>git add . 
C:\Hugo\naccoblog>git commit -m "download theme minimo"
```

##### テーマの適用
**config.toml(Hugoの設定ファイル)にテーマを追加します。**
```toml
baseURL = "http://example.org/"
languageCode = "ja"
title = "My New Hugo Site"
# add theme 
theme = "minimo"
```
`theme = "minimo"`でテーマの適用ができます。

ただ、どのテーマでも**config.tomlの設定はこれだけでは足りません！！**

テーマのGitHubリポジトリの**[exampleSite/config.toml](https://github.com/MunifTanjim/minimo/blob/master/exampleSite/config.toml)**にサンプルが置いてあるので、コピーして設定値を追記してください。だいたいexampleSite下にサンプルが置いてあります。

`baseURL`、`languageCode`、`title`などはHugoの共通設定なので、[公式ドキュメント](https://gohugo.io/getting-started/configuration/#example-configuration)を見ると書き方が載っています。

設定が終わったらもちろん、**git commit**
```
C:\Hugo\naccoblog>git add . 
C:\Hugo\naccoblog>git commit -m "set config theme minimo"
```

ここまででは、まだ何ができていて、何ができていないのかわからないと思います。

次回、いよいよ**ローカル環境でブログを表示してみます！**あと少しですよ！

次回：[コンテンツの作成、ローカルでプレビュー、1章まとめ≫](../hugo-firebase-12)

もし記事の内容でわからないところがあったら、★[質問箱](https://peing.net/ja/climbing_nacco?event=0)★に質問をください！Twitterで回答できるかはわかりませんが、このブログの記事か発表の糧にさせていただきます。