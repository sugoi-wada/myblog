---
layout: post
title: "Android Studioで覚えておくと便利なこと"
date: 2014-04-10 00:32:38 +0900
comments: true
categories: Android AndroidStudio
---
EclipseからAndroid Studioに乗り換えたのですが、設定画面の見方やショートカットの違い等にいろいろ戸惑ったのでメモ。

<!--more-->

※Macの場合でのみの説明です。Winの人はキーをいろいろと置き換えてみるとわかるかも。

## 設定画面

Menu > Preferences からAndroid Studio全体の設定が確認・変更できます。

#### マウスオーバーでドキュメントを表示する

Editor > Show quick doc on mouse over

#### 行番号・空白文字をつける

Editor > Appearance > Show line numbers

Editor > Appearance > Show whitespaces

#### ゲッター・セッター生成時にPrefixやSuffixを除く

通常、Androidのコーディング規約に沿って、privateなフィールドにはmをつけることがあるかと思います。しかし、getterやsetterを自動生成するときにgetmXX、setmXXとなっては困ります。これを防止するには下記を参照してください。

Code Style > Java > Code Generation > Name suffix にmを入れておく

#### プラグインの導入

Android Studioでは、Eclipseと同様プラグインがいくつかあります。Eclipseほどの量はありませんが、様々なプラグインがありますので、確認しておいて損はないでしょう。

Plugins から確認できます。ちなみに僕は

- Eclipse Code Formatter（Eclipse時代に利用していたFormatterを利用できるようにする）
- ideaVim（Vimのキーバインド）

の２つをいれています。

## ショートカット

デフォルトではショートカットのキーバインドが異なります。Eclipseのキーバインドに変更することも可能です。変更や確認がしたい場合は、設定画面のKeymapというところを参照してください。

僕はVim + Mac OS X 10.5+のキーバインドを利用していますが、以下にMac OS X 10.5+の場合のキーバインドを紹介します。

|名前|説明|ショートカットキー
|-|-|-
|Rename|名前の変更|⇧F6
|Auto Format|自動フォーマット|⌘⌥L
|Organize Imports|自動インポート|⌃⌥O
|Toggle Comment|選択行のコメント化・アンコメント化|⌘/ (Eclipseと同様)
|Exctract variable|ローカル変数の定義(Eclipseなら⌘2 L)|⌘⌥V
|Exctract field|フィールドの定義(Eclipseなら⌘2 F)|⌘⌥F
|Exctract method|メソッドの定義|⌘⌥M
|Move Tab|右のタブや左のタブに移動|⇧⌘[ or ]
|Run|アプリをビルドして実行する|⌃R

※随時追加するかも
