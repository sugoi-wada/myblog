---
layout: post
title: "Eclipse起動時のWorkspacesの選択ダイアログを表示する方法"
date: 2013-12-26 01:59:41 +0900
comments: true
categories: eclipse
---
Eclipseを起動した時にWorkspaceの選択ダイアログが表示されます。 しかし、デフォルトのWorkspaceに設定するにチェックを押すと、表示されなくなります。 その場合に再度表示する方法を説明します。

<!-- more -->

1.  Eclipse → 環境設定 → General → Startup and Shutdown → Workspaces に移動
2.  Prompt for workspace on startup にチェックを押す  

でも、Eclipse Junoはバグっていて、チェックしてても聞いてくれなかったりするみたいです。 なので、コマンドからクリーンして実行しましょう。

## Linuxの人

*   $ cd [Eclipseのディレクトリ]
*   $ ./eclipse -clean

## Macの人

*   $ cd [Eclipse.appのディレクトリ]/Contents/MacOS/
*   $ ./eclipse -clean

## 参考

[MacのEclipseを-cleanオプションで起動[mac,Eclipse]][1]

 [1]: http://info.yama-lab.com/?p=1335
