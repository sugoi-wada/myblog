---
layout: post
title: "Androidのスクリーンショットを簡単に撮る方法"
date: 2013-12-26 21:35:42 +0900
comments: true
categories: Android ShellScript
---
Android端末のスクリーンショットを撮る方法にはいくつかあります。今までEclipseのDDMSを使って撮影をしていたのですが、大量に撮りたいときにいちいちボタンを押して保存して．．．という作業が面倒だったので、adbコマンドからスクショを撮る方法を調べてみました。

<!-- more -->

## 一番単純な方法

```
$ adb shell screencap -p /sdcard/screen.png
$ adb pull /sdcard/screen.png
$ adb shell rm /sdcard/screen.png
```

でもこれ面倒だよね、ってことで、一発でやる方法はこちら。

## Ubuntuの人

    $ adb shell screencap -p | sed 's/\r$//' > screen.png

※ screen.pngはファイル名

## Mac OS Xの人

    $ adb shell screencap -p | perl -pe 's/\x0D\x0A/\x0A/g' > screen.png

※ screen.pngはファイル名

てな感じです．しかし、一気にバシャバシャ撮りたいのに、いちいちファイル名いじってコマンド叩くの面倒ですよね。てことで、初めてのシェルスクリプトを書いてみました。

``` sh ass.sh
FILENAME="screenshot_`date \"+%Y%m%d%H%M%S\"`.png"
while getopts n: opt
do
    if test $opt = "n"
    then
        FILENAME=${OPTARG}
    fi
done
adb shell screencap -p /sdcard/$FILENAME
adb pull /sdcard/$FILENAME
adb shell rm /sdcard/$FILENAME
```

### 簡単な説明

ass.shという名前で保存したのであれば、そのディレクトリに移動して、

    $ chmod 755 ass.sh

と、実行権限をシェルスクリプトに与えてあげます。そして、実行する場合は、

    $ ./ass.sh

とすれば実行できます。ファイル名は、「screenshot_[日時].png」です。
ファイル名を明示的に指定したい場合は、

    $ ./ass.sh -n xxxx.png

などど入力すればオッケーです。

どこのディレクトリでも実行できるようにしたければ、そのディレクトリにPATHを通すか、PATHの通っているbinディレクトリに入れましょう。

## 参考

[Grab Android screenshot to computer via ADB](http://blog.shvetsov.com/2013/02/grab-android-screenshot-to-computer-via.html)
