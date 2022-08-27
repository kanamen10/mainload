---
title: このブログを公開するまで
date: 2022-08-24T00:07:11+09:00
tags:
  - "ブログ"
  - "AWS"
categories:
  - "Web"
thumbnail: "images/20220827_1.png"
---

この記事は [#chillSAP](https://note.com/hashtag/chillSAP) [夏の自由研究2022](https://note.com/chillsap/n/n62740563aee0) 8/27の記事として執筆していますが、SAP関係者が書くSAPとは関係のない研究です。  
SAPの研究記事は今後上がります()

みなさま初めまして。でぶおぷ次郎([tnmrknm](https://twitter.com/tnmrknm))です。

某ホワイトコンサルティング企業でSAP関連の仕事をやっている猫です。
最近めっきり技術的なことをやらなくなってしまっていたのですが、このままではまずいと思い立ちブログを開設するに至りました。  
これまでもいくつかQiitaなどに技術記事を書いていたのですが、これからはこちらでツラツラと書き連ねていきます。

Zennでもいいんじゃないかとか、SAP Blogとかとも迷ったのですが、SAPやってる人はZennそんなに見ないだろうし、たぶんSAP関係ないこともそこそこ書くし、だったら自分でブログ立てればいいかということで、頑張ってアウトプットしていきます。

とはいえ思いつきでブログを公開するのにはそこそこ苦労したので、初回の今回は公開までの手はずをまとめてみました。

## ざっくり概要

前々から気になっていた[Hugo](https://gohugo.io/) + [AWS Amplify](https://aws.amazon.com/jp/amplify/)でブログを構成しました。
本当はWordpressを使ったり、あるいははてブロとかとも迷いましたが、どうせなら自分でサーバー代払って運営した方が長続きするだろうと思い、かつ多少新しい技術も触りたかったのでこの構成にしました。  
あんまり作り込む部分自体に時間をかけずに済んだのは、Hugoを使ったおかげでした。  
追い込んでアウトプットしていくスタイルを確立したい方には結構おすすめの手法なのでぜひ参考にしてみてください。

<!-- ![A building](/images/Note-Snippet.gif " ") -->

### AWS Amplify
AWS Amplifyは静的Webアプリケーションを構築・デプロイするための簡易的なCI/CDサービスやアプリケーションのホスティング環境を提供しているサービスです。
ブログ形式のような簡単な静的アプリを管理するのには一番ラクで簡単な手法かなということで選択しました。  
この辺で凝ってEKSとか使って構成したがると永久にブログそのものが出来ないと思い、今回は作ることに主眼を置いています。([Done is better than perfect](https://basicsbybecca.com/blog/done-is-better-than-perfect#:~:text=What%20Does%20%E2%80%9CDone%20is%20Better,your%20mental%20health%20and%20sanity.)の精神が大事)

サービス自体は東京リージョンで公開されていることもあり、最終的にレスポンスも良好でした。
コスト的にも、通信量で課金される形態なのでよほどアウトプットしなければ月に1$もかからず利用可能です。(コストは[こちら](https://aws.amazon.com/jp/amplify/pricing/))
アクセス数でコストが増大しないところも良いですね。


### Hugo
Hugoは「静的Webアプリケーション構築フレームワーク」です。
Hugoの大きな特徴としては、データベースを使用していないのでコンテンツは全てファイルで管理されています。
ブログなどの記事はMarkdown形式のファイルで構成されているため、Amplifyのようなサービス単体でデータベースサービスなしで構成が可能となっています。

フレームワークということで、いくつかの[テーマ](https://themes.gohugo.io/)が公開されています。
今回はブログ目的ということと、いくつかカスタマイズが必要なので情報の整っていそうな[mainroad](https://themes.gohugo.io/themes/mainroad/)を選択しました。

## 構築していく

### 前準備
一応公式にも構築の手順は公開されています。
- [Host on AWS Amplify](https://gohugo.io/hosting-and-deployment/hosting-on-aws-amplify/)

Pre-requisitesにも記載がある通り、AWSアカウントのセットアップが必要です。
AWSは[無料利用枠](https://aws.amazon.com/jp/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)もあるのでぜひセットアップしてみてください。  
あとはGitHubのアカウントもある方が良いです。
最初に忘れて躓いてしまいましたが、ローカルにてHugoアプリケーションをビルドできる環境が必須です！  
Macでやると簡単でしたがWindowsやLinuxでも可能なので忘れずローカルセットアップしましょう。

### カスタマイズ
下記のブログが日本語でかつ一通りカスタマイズについて触れられていて参考になりました。
- [Hugo によるブログ作成と mainroad テーマのカスタマイズ](https://terashim.com/posts/create-hugo-blog-and-customize-mainroad-theme/)

Hugoでベースのテーマをもとにカスタマイズをしていくことになるのですが、HugoではCSSやJSファイルに直接手を加えても良いのですが、元のレイアウトを崩してしまったり、将来のアップデート時に不具合が起こる可能性があるため非推奨のようです。  
そのため、Themesというディレクトリ配下にベーステーマとなるソースを置いておき、config.tomlに手を加えることでテンプレートやスタイル等をオーバライドすることができます。

### テーマカラーの変更
まずサイト全体のテーマカラーをベージュっぽい色、ハイライト色を水色に決定しました。こういった変更に関してもconfig.tomlで変更可能です。下記のように書き加えるとハイライト色を上書きしてくれます。
```yaml:config.toml
[Params]
  highlightColor = "#F5E6DB"
```

### フォント設定
Mainroad テーマのデフォルト設定では日本語フォントが設定されていないので、読みやすくするためにフォントを導入します。  
config.tomlに　.Params.customCSS フィールドを指定することでカスタムCSSファイルをインクルードできるようになっているので、これを利用して日本語フォントの設定を行います。

多くのブログでも導入されているようだったので[Noto Sans JP](https://fonts.google.com/specimen/Noto+Sans+JP) を導入しました。
あとは題字についてもデフォルトではなく、ゆるい感じを出したかったので[Moon Flower](https://www.dafont.com/moon-flower.font)を導入しています。 
それぞれのページから フォントのCSSファイルをダウンロードして static/css/ 配下に保存します。  
次にこれらのフォントを使用するためのカスタムCSSファイル static/css/custom.css を以下の内容で作成します。

```css:custom.css
body {
    font-family: 'Noto Sans JP', sans-serif;
}
.logo__title {
	font-size: 32px;
	font-size: 4rem;
	font-weight: 700;
	line-height: 1;
	color: #000;
	font-family: "moonflower", sans-serif;
}
```

これらのCSSファイルをインクルードするために、config.toml でCSSファイルのパスを指定しておきます。

```yaml:config.toml
[Params]
  customCSS = ["css/notosansjp.css", "css/custom.css"]
  customCSS = ["css/moonflower.css", "css/custom.css"]
```

### Twitterウィジェットの設置
Mainroad テーマでは layouts/partials/widgets/ にテンプレートファイルを保存することによって独自のサイドバーウィジェットを作成することができます。 よくあるブログのウィジェットとしてTwitterタイムラインの埋め込みを行いました。  

Twitter タイムライン をサイドバーに埋め込むには、まずTwitterの[ヘルプ記事](https://help.twitter.com/ja/using-twitter/embed-twitter-feed) に従って埋め込みコードを作成し、Hugo部分テンプレートファイル layouts/partials/widgets/twitter.html として保存します。 これでサイドバー用ウィジェット “twitter” として使えるようになります。
そして Hugo 設定ファイル config.toml で .Params.sidebar.widgets フィールドの値に "twitter" を追加すればサイドバーにウィジェットが追加されます。

今回はデフォルトであった検索機能などは消してシンプルなサイドバー構成にしています。
```yaml:config.toml
[Params.sidebar] # サイドバーの設定
  home = "right" # ホームページでは右に表示
  list = "right"  # 一覧ページでは右に表示
  single = false # 個別ページでは表示しない
  # ウィジェットを並べる
  widgets = ["recent", "categories", "taglist", "twitter"]
```


### プライバシーポリシー作成
加えたカスタマイズはこのくらいです。  
本当はGoogleアナリティクスを入れたり、SNSシェアボタンを入れたりしたいのですが、今回はGoogleアナリティクスの導入準備のためにプライバシーポリシーページだけ作成しました。
免責事項も含めて/privacy/のパスで公開し、Footer にリンクとして加えました。内容は[こちら](https://tnmrknm.net/privacy/)。

今回は何よりもまずは一回完成させることを目指していきます。追加機能はおいおい更新していこうかなと思います。

## Amplifyへデプロイ
ある程度できたところで、なにはともあれデプロイです。  
AWSコンソールへログインしてAmplifyへアクセスして、ウェブアプリケーションをホストに進みます。
![3](/images/20220827_3.png " ")

Gitプロバイダーなしでソースコードを直接ローカルからアップロードも可能ですが、Gitを繋げておけば今後はGit pushをトリガーにして勝手にビルド/デプロイまで走るので便利です。(CI/CDを語りたい欲が出そうですが、今回はさらっと流します)

GitHubを繋げるとAWS上にリポジトリ情報が出てきます。  
今回は先程カスタマイズしたMainroadテーマの資源を予めGitHubに上げておいてあるのでそちらを選択します。
![3](/images/20220827_4.png " ")

ここで私が躓いたポイント。  
ローカルでHugoの資源をビルドしてからPushしないとAmplify上でもビルドに失敗します！
公式の[Pre-requisites](https://gohugo.io/hosting-and-deployment/hosting-on-aws-amplify/)を読んでいれば分かるのですが、読み飛ばして痛い目に会いました。   
きちんとローカルでビルドされていると、ビルド/デプロイ時のパイプラインのコンフィグ上でも自動的にhugoの資源ということを認識して構築してくれます。
![3](/images/20220827_5.png " ")

ここでHugoの資源ということを認識していない場合は、恐らくビルドに失敗するので気をつけてください。  
あとはもう一つ注意点、Mainroadは最新版Hugoでのビルドに対応しているのでAmplifyでホストしているHugoのバージョンを最新版にしておきましょう。  
最新版指定は下記から可能です。
![3](/images/20220827_6.png " ")

もしビルドバージョン指定したい場合でも、パイプラインの中でHugoのコンテナをwgetしてこれば良さげ。  
色々試した結果、標準でバージョン最新化すれば結局問題なくビルドできたのですが備忘録的に残しておきます。
```yml:amplify.yml
version: 0.2
frontend:
  phases:
    build:
      commands:
        - wget https://github.com/gohugoio/hugo/releases/download/v0.101.0/hugo_0.101.0_Linux-64bit.tar.gz
        - tar -xf hugo_0.101.0_Linux-64bit.tar.gz hugo
        - mv hugo /usr/bin/hugo
        - rm -rf hugo_0.101.0_Linux-64bit.tar.gz
        - echo $(hugo version)
        - hugo
  artifacts:
    baseDirectory: public
    files:
      - '**/*'
  cache:
    paths: []
```

そうしたらあとはビルド開始！  
でビルド/デプロイが成功すれば無事に公開されることとなります。驚異的にかんたん。  
ドメインもRoute53で取得すればあっという間に紐付けられます。
Featureブランチを作れば検証環境も構築できるようです。なんてかんたんなんだ。

## おわり
ということでブログを作り、最後までSAPに全く関係なくお送りしてきました。(なんか繋げようと思ったけど無理だった)　　
今後はこのブログを通じて色々上げていくので、その準備でしたということで終わりにしようと思います。  
また記事を上げていくのでTwitterフォローお願いします〜