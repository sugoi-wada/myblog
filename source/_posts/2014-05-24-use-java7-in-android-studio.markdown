---
layout: post
title: "Android StudioでJava7を利用する"
date: 2014-05-24 18:01:10 +0900
comments: true
categories: Android AndroidStudio
---

Android SDK 22.6より、Java7が使えるようになりました。

<!--more-->

{% blockquote tools-note-22.6 http://developer.android.com/tools/sdk/tools-notes.html %}
Added support for Java 7 language features like multi-catch, try-with-resources, and the diamond operator. These features require version 19 or higher of the Build Tools. Try-with-resources requires minSdkVersion 19; the rest of the new language features require minSdkVersion 8 or higher.
{% endblockquote %}

上記をまとめると

+ Java7のmulti-catchやtry-with-resourcesやdiamond operatorのような機能がサポートされました
+ これらを使うにはBuild Toolsの19以上が必要
+ Try-with-resourcesはminSdkVersionが19以上
+ 残りの機能はminSdkVersionが8以上

ということです。詳しい機能の詳細に関しては[ここ](http://d.hatena.ne.jp/kirimin/20140310/1394469046)を参照してください。

## Android StudioでJava7を使えるようにする

本題です。とても簡単です。

1. SDK ManagerでBuild Toolsの19以上をインストールする（最新のもので良いでしょう）
1. 上記でインストールしたバージョンを app/build.gradle 内のbuildToolsVersionに書く
1. JDK1.7をインストール
1. Android Studioの File > Project Structure... をクリックし、JDKのパスを変更
![jdk-location](/images/androidstudio/jdk-location.png)
1. 該当プロジェクトの app/build.gradle に挿入

```
android{
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_7
        targetCompatibility JavaVersion.VERSION_1_7
    }
}
```

以上でOK
