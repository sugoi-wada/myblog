---
layout: post
title: "gitignoreを生成してくれるgitignore.ioの使い方"
date: 2014-02-11 20:48:35 +0900
comments: true
categories: gitignore
---
gitを利用する上で、管理下から除外の対象にしたい拡張子やディレクトリをgitignoreというファイル名で羅列することで、対象のファイルやディレクトリが管理下から外れてgitのコミットログが綺麗になるわけですが、その対象ファイルは言語や環境に応じて大体決まっています。それをgitで管理する度に毎回書くのは面倒ですが、gitignore.ioを使うとコマンドラインから自動生成してくれます。

<!--more-->

## インストール方法

[公式ページのここ](http://www.gitignore.io/cli)に書いてあることをコマンドラインから入力すれば完了です。

## 使い方

管理下のディレクトリでこんな感じで入力すればOK。

```
$ gi java,eclipse >> .gitignore
```

また、どのような言語、環境に対応しているのかを見るためには

```
$ gi list
```

と入力すれば、一覧が表示されるので、そこから探すと良いでしょう。

## 参考

[gitignore.io](http://www.gitignore.io/)
