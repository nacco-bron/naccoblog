---
title: "Hugo+Firebase+GitHub+CircleCIでブログ作る-1_2"
slug: "hugo-firebase-12"
authors: ["nacco"]
date: "2018-09-26"
description: "コンテンツの作成、ローカルでプレビュー、1章まとめ"
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

お元気ですか？[Nacco](https://twitter.com/climbing_nacco)ですn(╹ω╹n)

[前前回](../hugo-firebase-10)からHugoでこのブログを構築する**実際の手順**を書いています！

[前回](../hugo-firebase-11)はテーマを選んでダウンロード、設定するところまで進みました。

---

- [0章 ブログのシステム構成について](../hugo-firebase-00)
- **1章 Hugoで生成した静的サイトをローカル環境でプレビュー**
  - [1章で得られること、Hugoのインストール](../hugo-firebase-10)
  - [Hugoプロジェクトの作成、初期化、テーマの設定](../hugo-firebase-11)
  - **コンテンツの作成、ローカルでプレビュー、1章まとめ**
- 2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ
- 3章 Hugoで生成した静的サイトをGitHubとCircleCIを使ってFirebase Hostingに自動デプロイ
- 4章 テーマ選び、記事テンプレ作り

---
## 1章 Hugoで生成した静的サイトをローカル環境でプレビュー

#### コンテンツの作成
{{< figure src="/images/hugo-firebase-10/making-markdown.png" alt="コンテンツの作成" >}}

**Hello World!**的なサンプルコンテンツ(記事)を書きますよー。

コンテンツ(Markdownファイル)を新規作成します。`hugo new`コマンドを実行します。

`hugo new [path]`
```
C:\Hugo\naccoblog>hugo new posts/my-first-post.md
```

Markdownファイルを編集してみます。.mdファイルはここに作成されます。

`C:\Hugo\naccoblog\content\posts\my-first-post.md`

```yaml
---
date: "2018-09-26T02:44:13+09:00"
title: "My First Post | My New Hugo Site"
draft: true
---

Hello World!
テスト投稿です。

```

`---`で囲まれた部分はコンテンツの**メタ情報**、それ以下が**本文**になります。

まずは本文に何か適当に文章を入れてください。メタ情報については今回はそのままで大丈夫です。

メタ情報([Front Matter](https://gohugo.io/content-management/front-matter/#front-matter-formats))はyaml形式、toml形式、json形式で書くことができます。

- yaml形式は`---`で囲みます
- toml形式は`+++`で囲みます
- json形式は`{``}`で囲みます

メタ情報の項目

- date … 投稿日時
- title … 記事タイトル
- draft … 下書きフラグ

簡単そうだと思ったのに、一か所でも文法をミスるとジェネレータが文法エラーで止まることがあります。**けっこう苦戦しました**(;'∀')今回はあまりいじらず、シンプルな状態で一度やってみましょう。

最終的には原型(default.md)やテンプレートをカスタマイズしました。それについては4章で書いていきたいと思います！！

---
#### ローカルでHugoサーバを起動、プレビュー
{{< figure src="/images/hugo-firebase-10/previewing-locally.png" alt="ローカルでHugoサーバを起動、プレビュー" >}}

Hugoは**WEBサーバ(Hugoサーバ)を素早くローカルマシン上で起動**することができます。

Hugoサーバはコンテンツの変更を検知して、**メモリ上でサイトを生成して提供**します。(生成データはディスクに保存しません)
また、[LiveReload.js](https://gohugo.io/getting-started/usage/#livereload)を組込んであるので追加パッケージなしで、変更をリアルタイムに表示反映します。

どういうことかと言うと、Hugoサーバを起動した状態で、**Markdownファイルを編集し、保存すると即座にブラウザに反映して表示してくれます。**…まじで？

##### Hugoサーバの起動
`hugo server`コマンドでWEBサーバを起動します。
```
C:\Hugo\naccoblog>hugo server -D
```
{{% text s="0.8" color="#7058a3" %}}
**※1** 「**-D**」オプションはdraft(下書き)のコンテンツもプレビューします。メタ情報に「draft: **true**」と書いたら下書き(非公開)記事です。
{{% /text %}}
```
Building sites …
                   | JA | EN
+------------------+----+----+
  Pages            | 39 | 12
  Paginator pages  |  0 |  0
  Non-page files   |  0 |  0
  Static files     | 35 | 35
  Processed images |  0 |  0
  Aliases          | 11 |  1
  Sitemaps         |  2 |  1
  Cleaned          |  0 |  0

Total in 115 ms
Watching for changes in C:\Hugo\naccoblog\{content,data,layouts,static,themes}
Watching for config changes in C:\Hugo\naccoblog\config.toml
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```
##### WEBブラウザで表示

ブラウザで`http://localhost:1313/`にアクセスすると、サイトをプレビューできます。

トップページ`http://localhost:1313/`と
{{< figure src="/images/hugo-firebase-10/minimo-preview1.png" alt="サイトをプレビュー1" >}}
記事のページ`http://localhost:1313/posts/my-first-post/`です！
{{< figure src="/images/hugo-firebase-10/minimo-preview2.png" alt="サイトをプレビュー2" >}}

やったー！

ためしに、Markdownファイルを編集して、保存してみると…？
{{< figure src="/images/hugo-firebase-10/minimo-preview3.png" alt="サイトをプレビュー2" >}}

記事タイトルかわったー！

えっじゃあ、設定ファイル(config.toml)を編集して、保存してみると？
{{< figure src="/images/hugo-firebase-10/minimo-preview4.png" alt="サイトをプレビュー2" >}}

見にくいですが、メインメニュー「REPO」を「レポジトリ」に変更しました。これも即時反映でプレビューできます。
(リポジトリって打ちたかったの間違えた)

他にも、テンプレートなどをいじっても、即時反映されると思います。もし表示が変わらなかったらCtrl＋F5でブラウザのキャッシュをクリアして再読込みしてみてください。

---

### 1章のまとめ
{{< figure src="/images/hugo-firebase-10/ending-10.png" alt="とりあえず、ローカルで。" >}}
以上、1章では実際にブログを立てて、ローカル環境でプレビューする手順を紹介しました！

Hugoサーバ(リアルタイムなビルド、プレビュー)があるので、この記事も内容が固まったら**4割ぐらいはプレビューしながら文章を調整しています。**

1行足したのを**25 ms**で反映してくれます。涙が出そうです。

さて、次の章ではいよいよ静的ホスティングにデプロイしていきます！

次回：2章 Hugoで生成した静的サイトをFirebase Hostingに手動デプロイ

もし記事の内容でわからないところがあったら、★[質問箱](https://peing.net/ja/climbing_nacco?event=0)★に質問をください！Twitterで回答できるかはわかりませんが、このブログの記事かインフラ勉強会での発表の糧にさせていただきます。

### 参考資料
- [Git for Windows導入方法＆初期設定まとめ](http://vdeep.net/git-for-windows)
- [気をつけて！Git for Windowsにおける改行コード](https://qiita.com/uggds/items/00a1974ec4f115616580)
- [わかばちゃんと学ぶ Git使い方入門/湊川あい](http://amzn.asia/d/bnr4b23)
- [Installing Chocolatey](https://chocolatey.org/install#install-with-powershellexe)
- [Install Hugo | Hugo](https://gohugo.io/getting-started/installing)
- [Installing Hugo on Windows | Hugo(YouTube)](https://youtu.be/G7umPCU-8xc)
- [Installing Hugo on Windows | Hugo](https://gohugo.io/getting-started/installing#windows)
- [【Windows版】初心者のための！環境変数の基礎とPathの設定方法](https://yukiwet.com/setpath/)
- [「PATH を通す」の意味をできるだけわかりやすく説明する試み](https://qiita.com/sta/items/63e1048025d1830d12fd)
- [Go言語製のWebサイトエンジンHugoの機能整理 - Front Matter](http://tbpgr.hatenablog.com/entry/2015/08/12/224727)
- [日時のフォーマット（ISO 8601）](https://qiita.com/kidatti/items/272eb962b5e6025fc51e)
- [Howto: Standardize Date Format in HTML Attributes](https://discourse.gohugo.io/t/howto-standardize-date-format-in-html-attributes/758)