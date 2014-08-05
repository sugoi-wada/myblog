---
layout: post
title: "Android StudioでNDKをビルドする"
date: 2014-08-02 16:34:26 +0900
comments: true
categories: Android AndroidStudio NDK
---

Android Studioで、共有モジュール（.so）を生成し、利用する方法です。

<!--more-->

Android Studioを用いたNDKのサポートは[公式ページ](https://developer.android.com/sdk/installing/studio.html)によれば、`Coming soon`らしいです（投稿日時点）。

![](/images/androidstudio/feature.png)

ところが、Android Studioを使うのに欠かすことができないビルド管理ツールのGradleは、ver 0.8.0よりNDKのビルド機能が追加されています（[New Build System](http://tools.android.com/tech-docs/new-build-system)）。

実際にJNIを用いてやってみたら、共有モジュールが作れましたので説明します。

* Android Studio 0.8.4
* Android Gradle 0.12

で説明しています。

まず、[こちら](https://developer.android.com/tools/sdk/ndk/index.html)からAndroid NDKをインストールします。お好きなところに解凍してください。そして、プロジェクトの`local.properties`内にパスを記述します。

```
ndk.dir=/path/to/android-ndk-r10c
```

次にJNIのCのコードを`app/src/main/jni/`配下に保存します。Android Studioから新規にディレクトリやファイルを作成して書いても構いません。

![](/images/androidstudio/jni-dir.png)

次に、`app/build.gradle`内の`android.defaultConfig`内にNDKの設定を記述します。

```
android {
    ....
    defaultConfig {
        ....
        ndk {
            moduleName "modulename"
            stl "gnustl_shared"
            abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
            ldLibs "log"
        }
    }
}
```

* moduleName : モジュール名
* stl : 共有ライブラリを作成する場合は "gnustl_shared"を指定
* abiFilters : CPUアーキテクチャを指定
* ldLibs : ライブラリを用いる場合はここに指定

以上の設定でアプリを実行してみます。すると、Gradleの実行時に`ndk-build`が行われ、共有モジュールが生成されます（ビルドのみでは`ndk-build`は行われません）。

これで完成！でもいいのですが、実行の度に毎回`ndk-build`をコールするのはコストがかかりますし、CIでビルドしている場合は、CI環境にNDKがインストールされていないとビルドに失敗してしまいます。なので、最初に一度だけ`ndk-build`を行い、得られた共有モジュールを組み込みましょう。

一度アプリを実行した場合は既に`ndk-build`が完了しているので、共有モジュールを取りに行きましょう。共有モジュールは`app/build/intermediates/ndk/debug/lib/`配下にあります（Android Studioからintermediatesディレクトリが見えないように設定されていますので、ファイラーかターミナルで確認してください）。

![](/images/androidstudio/ndk-build-dir.png)


`app/build.gradle`のabiFiltersに記述したCPUアーキテクチャごとに作成されていますので、それぞれ`app/src/main/jniLibs/`配下にコピペします。

![](/images/androidstudio/jnilibs-dir.png)

その後、`ndk-build`を毎回コールしないように、`app/build.gradle`内の`android.sourceSets.main`内に`jni.srcDirs = []`と記述します。

```
android {
    ....
    sourceSets {
        main {
        	....
            jni.srcDirs = [] //ndk-buildを毎回コールするのを避ける
        }
    }
}
```

これで、今後のビルドでは`ndk-build`をコールせず、`jniLibs`内の共有モジュールを使ってくれるようになります。お疲れ様でした。
