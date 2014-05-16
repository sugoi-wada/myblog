---
layout: post
title: "TestFlightの代わりとしてTestFairyを使ってみた"
date: 2014-03-10 22:20:40 +0900
comments: true
categories: Android
---
AndroidアプリやiOSアプリのベータテストサービスとして仕事でもプライベートでも利用していた[TestFlight](http://testflightapp.com/)ですが、米Apple社がTestFlightを運営しているBurstly社を買収したことにより、Androidのサポートが2014年4月21日に終了することが明らかになりました。

<!--more-->

これにより、終了までに代替サービスを探す必要がでてきました。有名なサービスで言えば、例えば[DeployGate](https://deploygate.com/)があります。mixi社が運営しているサービスで、日本語もバッチリ対応しています。十分使えるサービスだとは思いますが、SDKを入れ替えたり、いくつかのコードを書き換えなくてはなりません。

今回紹介するTestFairyは、TestFlightのSDKをそのまま使えます。さらに、コードを一切書き換える必要がありません。TestFlight利用者にとってはかなり楽に移行ができますのでオススメ。さらに、セッションごとのスクリーンのビデオ撮影、時間ごとのCPU使用率、メモリの消費量、アプリのログなどが記録され、確認することができる優れものです。

## TestFairyを使ってみる

[https://www.testfairy.com/](https://www.testfairy.com/)にアクセスし、SIGN UPし、ログインします。

そして、上部メニューのUPLOADから~~TestFlight SDKを導入済みの~~(あってもなくても関係ないようです)apkファイルをアップロードします。ここで、記録したいセンサーの情報をチェックします。アプリのパフォーマンスが悪くなるので、センサーは最小限にとどめておきましょう。他の項目は、アイコンをTestFairy用アイコンにする（アイコンの下端にTestFairyアイコンがついたものになる）設定や、アップデート時に自動的にアップデートする設定です。

![UPLOAD SCREEN](/images/testfairy/upload.png)

### テスト用アプリをインストールする

アプリのインストール（配布）方法がTestFlightなどとは異なり、URLもしくはメール、APK配布の3種類あります。今回はTestFairyがオススメするメールでのインストールを行ってみます。

![Overview](/images/testfairy/overview.png)

アップロード後（もしくはOverview）の画面の左にあるSEND APP VIA E-MAILをクリック。

![INVITE SCREEN](/images/testfairy/invite.png)

追加したいユーザのメールアドレスを追加し、Invite Selected Testersを押すと、インストール先のURLやQRコードが添付されたメールが送信されます。

#### ログインしなければインストールできない設定にする

デフォルトはパブリックリンクが送られてくるかつ、テスターを募集できるように公開用リンクもオープンな状態になっていますので、オフにしておいた方がいいでしょう。

まずはアプリメニューのSettings画面へ。

![Settings](/images/testfairy/settings.png)

Testers Strict ModeのEnablesにチェックを打ちます。こうすると、メールで招待した相手には、ログイン用URLが送られ、ログインしたユーザのメールアドレスが招待済みであった場合にのみインストールができるようになります。

続いてはBeta Community画面へ。

![Beta Community](/images/testfairy/community.png)

Beta TypeのClosed betaにチェックを打ちます。こうすると、一般のユーザはメールアドレスを承認されないとアプリをインストールできなくなります。デフォルトはパブリックURLを踏めば誰でもテスト用アプリをインストールできてテストに参加してしまえる状態ですので、こちらのほうが安全です。

### Facebook APIやGoogle APIを使う時

Settings画面のSignatureをアプリに合わせて変更すると利用できます。

### ログを取ってみる

まずは、Androidの設定画面から、「提供元不明のアプリ」にチェックします。そして、先ほどのメールアドレスからアプリをダウンロードし、インストールすると、やたら権限の多いアプリに進化しています。それは、TestFairyがログを取るために必要な権限がもりもり追加されているからです。アプリを起動し、いろいろ動かしてみます。その後、TestFairyのアプリのOverview画面を見てみると、Sessionが追加されていることがわかります。

![session](/images/testfairy/session.png)

セッションを1つ選ぶと、記録されたログを確認することができます。動画をとっている場合はは自動的に再生されます。この辺りは[デモ](https://app6.testfairy.com/projects/50-groupshot/builds/4461)をいろいろ動かしてみてください。

### 使用感とまとめ

TestFlightを使っていたアプリはTestFairyの導入がすごく楽です。というかWeb上で全て完結します。さらに取れるログも豊富で見やすいです。ただし録画はあまりサクサクと見れるわけではないので、わざわざ撮る必要はないかもしれません。あと、OpenGLのViewは録画できますが、カメラのViewは真っ黒な画面になります。

個人的にはすごくオススメしますので、是非一度使ってみてはどうでしょうか。
